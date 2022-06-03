<script lang="ts">
	import { initializeApp } from 'firebase/app';
	import {
		getAuth,
		signInWithEmailAndPassword,
		signOut,
		type User,
		onAuthStateChanged
	} from 'firebase/auth';
	import { onMount } from 'svelte';
	let email = '';
	let password = '';
	let user: User | null;
	const firebaseConfig = {
		apiKey: 'AIzaSyAYLZXsNWkhqwAG_hsXQhMZOAQ5TW_M9tI',
		authDomain: 'fir-auth-tutorial-6a0a0.firebaseapp.com',
		projectId: 'fir-auth-tutorial-6a0a0',
		storageBucket: 'fir-auth-tutorial-6a0a0.appspot.com',
		messagingSenderId: '1096742130021',
		appId: '1:1096742130021:web:e53b3b2c2118918f02518c'
	};

	const app = initializeApp(firebaseConfig);

	const login = () => {
		const auth = getAuth(app);
		signInWithEmailAndPassword(auth, email, password).catch((error) => {
			const errorCode = error.code;
			const errorMessage = error.message;
			console.log(errorCode, errorMessage);
		});
	};
	const logout = async () => {
		const auth = getAuth(app);
		signOut(auth);
	};

	onMount(async () => {
		const auth = getAuth(app);
		onAuthStateChanged(auth, (newUser) => {
			user = newUser;
		});
	});
</script>

{#if user}
	<p>Signed in!</p>
	<button on:click={logout}>Logout</button>
{:else}
	<input type="email" id="email" placeholder="email" bind:value={email} />
	<input type="password" id="password" placeholder="password" bind:value={password} />
	<button on:click={login}>Login</button>
{/if}
