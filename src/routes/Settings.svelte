<script>
    import { getPath } from "./utils";
    import { app, musicGenres } from "./app";

    export let showSettingsPopup;
    let prevState = $app.playMusic;
    let prevGenre = $app.music.genre;

    const close = () => {
        showSettingsPopup = false;
        // Reload the music if we change our music settings
        $app.music.reload =
            $app.music.genre != prevGenre || prevState != $app.playMusic;
        prevGenre = $app.music.genre;
        prevState = $app.playMusic;
    };
</script>

{#if showSettingsPopup}
    <div class="backdrop">
        <dialog open>
            <button class="close" on:click={() => close()}>
                <img src={getPath("close.svg")} alt="Close icon" />
            </button>

            <div>
                <label>
                    <input type="checkbox" bind:checked={$app.playRingtone} />
                    Ring on session switch
                </label>
                <label>
                    <input
                        type="checkbox"
                        bind:checked={$app.showNotification}
                    />
                    Notify on session switch
                </label>
                <label>
                    <input type="checkbox" bind:checked={$app.playMusic} />
                    Play background music
                </label>
            </div>

            {#if $app.playMusic}
                <label>
                    Music choice:
                    <select bind:value={$app.music.genre}>
                        <option value={musicGenres.lofi}> Lofi beats </option>
                        <option value={musicGenres.classical}>
                            Classical
                        </option>
                        <option value={musicGenres.nature}> Nature </option>
                        <option value={musicGenres.noise}> Noise </option>
                    </select>
                </label>
            {/if}

            <h3>Durations in minutes</h3>
            <div class="durations">
                <div class="duration">
                    <p>Work session</p>
                    <input type="number" bind:value={$app.durations[0]} />
                </div>
                <div class="duration">
                    <p>Short break</p>
                    <input type="number" bind:value={$app.durations[1]} />
                </div>
                <div class="duration">
                    <p>Long break</p>
                    <input type="number" bind:value={$app.durations[2]} />
                </div>
            </div>
        </dialog>
    </div>
{/if}

<style>
    @media only screen and (max-width: 550px) {
        dialog {
            top: 50%;
            border: none;
            width: 80%;
            height: fit-content;
            margin: 0 auto;
            border-radius: 10px;
            transform: translateY(-80%);
        }

        .close img {
            width: 15px;
            height: 15px;
        }
    }

    @media only screen and (min-width: 550px) {
        dialog {
            top: 50%;
            border: none;
            width: 30%;
            height: fit-content;
            margin: 0 auto;
            border-radius: 10px;
            transform: translateY(-80%);
        }

        .close img {
            width: 25px;
            height: 25px;
        }
    }

    .backdrop {
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        position: fixed;
        z-index: 1;
        background-color: rgba(0, 0, 0, 0.5);
    }

    .close {
        border: none;
        outline: none;
        cursor: pointer;
        float: right;
        background-color: rgba(0, 0, 0, 0);
    }

    .durations {
        display: flex;
        flex-direction: row;
        margin-top: -15px;
    }

    .duration {
        width: 60px;
        text-align: center;
        margin-right: 15px;
    }

    label {
        margin-top: 10px;
        font-size: 16px;
        display: block;
    }

    input {
        border: none;
        outline: none;
        border-bottom: 1px solid #67b26f;
    }

    input:active,
    input:hover {
        background-color: #fcfcfc;
    }

    input[type="number"] {
        width: 50px;
        outline: none;
    }

    input[type="checkbox"] {
        width: 18px;
        height: 18px;
        margin-right: 3px;
        cursor: pointer;
        vertical-align: middle;
        position: relative;
        bottom: 1px;
    }
</style>
