<script>
    import { slide } from "svelte/transition";
    import { onMount } from "svelte";

    let code = ["", "", "", "", "", ""];
    let placeHolder = ["_", "", "", "", "", ""];
    let OTPkey = "123456";
    let remainingDigits = 6;
    let showError = false;
    let showAnimation = false;

    $: if (code.join("") === OTPkey) {
        showAnimation = true;
        setTimeout(() => (showAnimation = false), 1000);
    }

    onMount(() => {
        const inputWrapper = document.getElementById(`code-wrapper-0`);
        inputWrapper.style.border = "2px solid #4b9ce2";
        document.getElementById("code-0").focus();
    });

    const handleInput = (index, event) => {
        const value = event.target.value;
        if (value.length === 1 && index < 5) {
            const nextInput = document.getElementById(`code-${index + 1}`);
            const nextInputWrapper = document.getElementById(
                `code-wrapper-${index + 1}`,
            );
            nextInput.focus();
            nextInputWrapper.style.border = "2px solid #4b9ce2";
        }
        if (event.target.value) {
            code[index] = value;
            placeHolder[index + 1] = "_";
            const tempOTP = parseInt(code.join(""))?.toString();
            event.target.classList.add("slide-down");
            if (tempOTP.length === 6 && `${tempOTP}` !== OTPkey) {
                document
                    .querySelectorAll(".code-input-wrapper")
                    .forEach((input) => {
                        input.style.border = "2px solid red";
                    });
                document.querySelectorAll(".code-input").forEach((item) => {
                    item.style.color = "red";
                });
                showError = true;
            }
        } else {
            event.target.classList.remove("slide-down");
        }

        event.target.classList.add("slide-up");
        updateRemainingDigits();
    };

    const updateRemainingDigits = () => {
        remainingDigits = 6 - code.filter((num) => num).length;
    };

    const handleKeyDown = (index, event) => {
        if (event.key === "Backspace") {
            checkCode();
            if (!code[index] && index > 0) {
                document.getElementById(`code-${index - 1}`).focus();
                placeHolder[index] = "";
            } else {
                code[index] = "";
                placeHolder[index + 1] = "";
                event.target.value = "";
                event.target.style.border = "";
                event.target.style.color = "";
                event.target.classList.remove("filled");
            }
            updateRemainingDigits();
            if (showError) {
                showError = false;
            }
            if (parseInt(code.join(""))?.toString()?.length === 5) {
                document
                    .querySelectorAll(".code-input-wrapper")
                    .forEach((input) => {
                        input.style.border = "2px solid #4b9ce2";
                    });
                document.querySelectorAll(".code-input").forEach((input) => {
                    input.style.color = "#4b9ce2";
                });
            }
        } else if (isNaN(event.key)) {
            event.preventDefault();
        }
    };

    const checkCode = () => {
        const enteredCode = code.join("");
        if (remainingDigits === 0) {
            if (enteredCode !== OTPkey) {
                document
                    .querySelectorAll(".code-input-wrapper")
                    .forEach((input) => {
                        input.style.border = "2px solid red";
                        input.style.color = "red"; // Set text color to red
                    });
            } else {
                document
                    .querySelectorAll(".code-input-wrapper")
                    .forEach((input) => {
                        input.style.border = "2px solid #4b9ce2";
                        input.style.color = "#4b9ce2"; // Set text color to blue
                    });
            }
        }
    };
</script>

<div class="auth-card" in:slide={{ x: 1000, duration: 500 }}>
    <div class="lockIconWrapper">
        <div
            class={`lockIconBg ${code.join("") === OTPkey ? "unlocked" : ""}`}
            class:showAnimation
            class:showError
        >
            <img
                src={code.join("") === OTPkey
                    ? "/unlock.png"
                    : showError
                      ? "red-lock.png"
                      : "/lock.png"}
                class="lockImg"
                alt="Lock Icon"
            />
        </div>
    </div>

    <h1>Sample App</h1>
    <p>Enter 6-digit code from your two-factor authenticator app.</p>
    <div class={showError ? "shake" : ""}>
        <div class="code-inputs">
            {#each code as digit, index}
                <div id={`code-wrapper-${index}`} class="code-input-wrapper">
                    <input
                        id={`code-${index}`}
                        type="text"
                        inputmode="numeric"
                        placeholder={placeHolder[index]}
                        maxlength="1"
                        bind:value={code[index]}
                        on:input={(e) => handleInput(index, e)}
                        on:keydown={(e) => handleKeyDown(index, e)}
                        class="code-input"
                    />
                </div>
            {/each}
        </div>

        {#if remainingDigits > 0}
            <div class="status">
                {remainingDigits} digits left
            </div>
        {:else if code.join("") === OTPkey}
            <button
                class="submit-btn btn-primary animated-slide"
                style="width: 100%;"
            >
                Let's Go
            </button>
        {:else}
            <button
                on:click={checkCode}
                class="submit-btn-danger"
                style="width: 100%;"
            >
                Wrong code!
            </button>
        {/if}
    </div>
    <div class="actualOTP">Note: Actual OTP is 123456</div>
</div>

<style>
    .slide-down {
        animation: slideDown 0.5s ease forwards;
    }

    @keyframes slideDown {
        0% {
            opacity: 0;
            transform: translateY(-20px);
        }
        100% {
            opacity: 1;
            transform: translateY(0);
        }
    }

    :global(body) {
        margin: 0;
        background-color: #e3f3ff;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        font-family:
            system-ui,
            -apple-system,
            BlinkMacSystemFont,
            "Segoe UI",
            Roboto,
            Oxygen,
            Ubuntu,
            Cantarell,
            "Open Sans",
            "Helvetica Neue",
            sans-serif;
    }

    .auth-card {
        width: 500px;
        padding: 50px 40px;
        background-color: white;
        border-radius: 15px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        text-align: center;
    }

    .icon {
        font-size: 2rem;
        background-color: #d8ebf9;
        width: 50px;
        height: 50px;
        margin: 0 auto;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #4b9ce2;
        margin-bottom: 10px;
    }

    /* Title and Subtitle */
    h1 {
        font-size: 1.5rem;
        color: #25659d;
        margin: 10px 0 5px;
    }

    p {
        color: #777;
        font-size: 1.1rem;
        margin: 10px 0 20px;
    }

    /* Code Input Fields */
    .code-inputs {
        display: flex;
        justify-content: center;
        gap: 8px;
        margin: 15px 0;
    }

    .code-input {
        width: 50px;
        height: 60px;
        font-size: 1.5rem;
        text-align: center;
        border: none;
        outline: none;
        color: #4b9ce2;
    }

    .code-input-wrapper {
        border: 2px solid #82888e;
        border-radius: 8px;
        outline: none;
    }

    /* Shake Animation */
    .shake {
        animation: shake 0.3s ease;
    }
    .actualOTP {
        margin-top: 20px;
        color: #6782bb;
        font-weight: 500;
    }
    @keyframes shake {
        0%,
        100% {
            transform: translateX(0);
        }
        25% {
            transform: translateX(-5px);
        }
        50% {
            transform: translateX(5px);
        }
        75% {
            transform: translateX(-5px);
        }
    }

    /* Status Message */
    .status,
    .submit-btn,
    .submit-btn-danger {
        font-size: 1.2rem;
        border-radius: 8px;
        max-width: 300px;
        margin: auto;
        padding: 14px 8px;
    }
    .status {
        color: #999;
        background-color: #eaeaea;
    }

    /* Button Styles */
    .submit-btn {
        color: white;
        border: none;
        background: linear-gradient(90deg, gray 50%, #4b9ce2 50%);
        background-size: 200%;
        background-position: 0%;
        transition: background-position 0.5s ease;
    }
    .submit-btn-danger {
        color: white;
        background-color: #ff4d4d;
    }
    /* Animation for "Let's Go" Button */
    .animated-slide {
        animation: slide-bg 1s forwards;
    }

    @keyframes slide-bg {
        from {
            background-position: 0%;
        }
        to {
            background-position: 100%;
        }
    }

    /* Button State Colors */
    .btn-primary {
        background-color: #4b9ce2;
    }
    input {
        padding: 8px;
        font-size: 16px;
        border: 1px solid #ccc;
        transition: all 0.3s ease; /* Transition for the entire input, including placeholder */
        margin: 0;
        border-radius: 8px;
        background: transparent;
        caret-color: transparent;
    }

    input::placeholder {
        color: #4b9ce2;
        width: 100%;
        opacity: 1;
        transition:
            opacity 0.3s ease,
            transform 0.5s ease;
    }

    /* On focus, change the placeholder color */
    input:focus::placeholder {
        transform: translateY(-3px);
    }

    .lockImg {
        width: 64px;
        height: 64px;
    }
    .lockIconBg {
        align-items: center;
        display: flex;
        margin: auto;
        justify-content: center;
    }
    .lockIconBg img {
        z-index: 10;
    }
    .lockIconBg {
        /* display: inline-block; */
        width: 100px;
        height: 100px;
        border-radius: 50%;
        /* margin: 0 5px; */
        position: relative;
        background-color: #d1e9ff;
    }

    .lockIconBg::before {
        content: "";
        position: absolute;
        right: 0;
        width: 100%;
        height: 100%;
        background-color: inherit;
        border-radius: 50%;
        z-index: 1;
        opacity: 0;
    }
    .unlocked {
        background: #b8f5bf;
    }
    .lockIconBg.showAnimation {
        background: #b8f5bf;
    }

    .lockIconBg.showError {
        background: #ffcdcd;
    }

    /* Add animation only when showAnimation is true */
    .lockIconBg.showAnimation::before {
        animation: ripple 1.5s ease-out;
        opacity: 1;
    }

    .lockIconBg.showError::before {
        animation: ripple 1.5s ease-out;
        opacity: 1;
    }

    @keyframes ripple {
        from {
            opacity: 1;
            transform: scale(0);
        }
        to {
            opacity: 0;
            transform: scale(2);
        }
    }
</style>
