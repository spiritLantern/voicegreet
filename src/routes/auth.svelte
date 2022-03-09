<script>
    import {onMount} from "svelte";

    onMount( () => {
        handleClientLoad();
    })

    // Client ID and API key from the Developer Console
    const CLIENT_ID = '756964178199-6d7ggko7s7a9njq566ur3tfvhlrntri7.apps.googleusercontent.com';
    const API_KEY = 'AIzaSyDmjXzTE7uTRH4O_1r64P5vItd1nOY85K4';

    // Array of API discovery doc URLs for APIs used by the quickstart
    const DISCOVERY_DOCS = ["https://sheets.googleapis.com/$discovery/rest?version=v4"];

    // Authorization scopes required by the API; multiple scopes can be
    // included, separated by spaces.
    const SCOPES = "https://www.googleapis.com/auth/spreadsheets";

    let isAuthorizeButton = false;
    let isSignOutButton = false;

    /**
     *  On load, called to load the auth2 library and API client library.
     */
    function handleClientLoad() {
        gapi.load('client:auth2', initClient);
    }

    /**
     *  Initializes the API client library and sets up sign-in state
     *  listeners.
     */
    function initClient() {
        gapi.client.init({
            apiKey: API_KEY,
            clientId: CLIENT_ID,
            discoveryDocs: DISCOVERY_DOCS,
            scope: SCOPES
        }).then(function () {
            // Listen for sign-in state changes.
            gapi.auth2.getAuthInstance().isSignedIn.listen(updateSigninStatus);

            // Handle the initial sign-in state.
            updateSigninStatus(gapi.auth2.getAuthInstance().isSignedIn.get());
            console.log(gapi.auth2.getAuthInstance().isSignedIn.get());
        }, function(error) {
            console.log(JSON.stringify(error, null, 2));
        });
    }

    /**
     *  Called when the signed in status changes, to update the UI
     *  appropriately. After a sign-in, the API is called.
     */
    function updateSigninStatus(isSignedIn) {
        if (isSignedIn) {
            isAuthorizeButton = false;
            isSignOutButton = true;
            console.log(isSignedIn);
        } else {
            isAuthorizeButton = true;
            isSignOutButton = false;
            console.log(isSignedIn);
        }
    }

    /**
     *  Sign in the user upon button click.
     */
    function handleAuthClick(event) {
        gapi.auth2.getAuthInstance().signIn();
    }

    /**
     *  Sign out the user upon button click.
     */
    function handleSignoutClick(event) {
        gapi.auth2.getAuthInstance().signOut();
    }

</script>

<div class="container-2xl px-1.5 grid flex items-center justify-items-center h-screen m-0">
    {#if (isAuthorizeButton)}
        <button class="px-5 py-5 bg-sky-500  shadow-lg rounded-lg text-white hover:bg-sky-700" on:click={handleAuthClick}>Sign In / Authorize to Google Access</button>
    {:else }
        <button class="px-5 py-5 bg-red-500  shadow-lg rounded-lg text-white hover:bg-red-700" on:click={handleSignoutClick}>Sign Out Authorize to Google Access</button>
    {/if}
</div>

