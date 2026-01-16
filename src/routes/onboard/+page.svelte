<script>
    import { onMount } from "svelte";
    import { fly, slide } from "svelte/transition";
    import { base } from "$app/paths";
    let Peer;

    let toggle = $state(false);
    

    onMount(() => {
        setTimeout(() => {
            toggle = true;
        }, 100);
    })

    let code = $state("");

    let peer;
    onMount(async () => {
        const pkg = await import("peerjs");
        Peer = pkg.default;
        
        let mainPeer = sessionStorage.getItem("mainPeer")
        let groupPeer = sessionStorage.getItem("groupPeer");
        if (mainPeer != null && groupPeer != null) {
            peer = new Peer(mainPeer + "-" + groupPeer);
            let conn = peer.connect(mainPeer);
            conn.on("open", function() {
                conn.send([mainPeer, groupPeer, "init check"]);

                conn.on("data", function(data) {
                    if (data[0] == mainPeer && data[1] == "main server") {
                        if (data[2] == "handshake") {
                            console.log("Handshake successful");
                        }
                    }
                })

                conn.on("error", function(err) {
                    console.log(err);
                })
            })


        }
        else {
            window.location.href = base;
            return 0;
        }
    })

    

    /*
    function processCode() {
        let peerConnect = "codingcafe-" + code.substring(0,1) + "-";
        peerConnect += "" + (parseInt(code.substring(1))*67);
        sessionStorage.setItem("mainPeer", peerConnect);
        console.log(peerConnect);
    }
        */
</script>
<svelte:head>
    <title>Vogel</title>
</svelte:head>

<h1>VOGEL</h1>
<p style="margin-bottom: 50px;">Coding Café Competition Portal</p>


