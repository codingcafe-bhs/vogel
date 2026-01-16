<script>
    import { onMount } from "svelte";
    import { fly, slide } from "svelte/transition";
    import { base } from "$app/paths";

    let toggle = $state(false);
    onMount(() => {
        setTimeout(() => {
            toggle = true;
        }, 100);
        setTimeout(() => {
            document.getElementById("codeInput").focus();
        }, 1100)
    })

    let code = $state("");
    let group = $state("");
    let numbers = ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"]
    let errorMessage = $state("")

    function processCode() {
        errorMessage = "";
        if (code.length != 5) {
            code = "";
            errorMessage = "(Invalid Code)";
            setTimeout(() => {
                document.getElementById("codeInput").focus();
            }, 1000)
            return false; 
        }
        for (let i = 0; i < code.length; i++) {
            if (numbers.indexOf(code.substring(i, i+1)) == -1) {
                code = "";
                errorMessage = "(Invalid Code)";
                setTimeout(() => {
                    document.getElementById("codeInput").focus();
                }, 1000)
                return false; 
            }
        }
        let peerConnect = "codingcafe-" + code.substring(0,1) + "-";
        peerConnect += "" + (parseInt(code.substring(1))*67);
        sessionStorage.setItem("mainPeer", peerConnect);
        sessionStorage.setItem("groupPeer", group);
        window.location.href = base + "/onboard"
        console.log(peerConnect);
    }
</script>
<svelte:head>
    <title>Vogel</title>
</svelte:head>

<h1>VOGEL</h1>
<p style="margin-bottom: 50px;">Coding Café Competition Portal</p>

{#if toggle}
<form transition:fly={{y:200}} onsubmit={processCode}>
    <h2 style:font-family="Gabarito, Space Grotesk, Montserrat, Futura;">Enter Competition Code</h2>
    <p><i>{errorMessage}</i></p>
    <input required bind:value={code} style="margin-bottom: 30px;" class="large" id="codeInput"/>
    <h2 style:font-family="Gabarito, Space Grotesk, Montserrat, Futura;">Enter Group Identifier</h2>
    <input required bind:value={group} style="margin-bottom: 30px;" class="large" id="groupInput"/>

    {#if code.length == 5 && group.length > 0}<input transition:slide style="margin-bottom: 30px; font-size: 18px;" type="submit" value="Continue">{/if}
</form>
{/if}
