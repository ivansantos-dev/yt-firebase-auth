<script lang="ts">
	import { initializeApp } from 'firebase/app';
	import {
		getAuth,
		signInWithEmailAndPassword,
		signOut,
		type User,
		onAuthStateChanged,
		signInWithPopup,
		GoogleAuthProvider,
		GithubAuthProvider
	} from 'firebase/auth';
	import { onMount } from 'svelte';

	let email = '';
	let password = '';
	let user: User | null;

	const firebaseConfig = {
		apiKey: 'AIzaSyB9XxiwZj0Oafuk04kGxPNabhR0-NkZWVQ',
		authDomain: 'yt-v-b0b56.firebaseapp.com',
		projectId: 'yt-v-b0b56',
		storageBucket: 'yt-v-b0b56.appspot.com',
		messagingSenderId: '498551643832',
		appId: '1:498551643832:web:400224298197c60ee15e4d'
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

	const loginWithGoogle = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GoogleAuthProvider());
	};

	const loginWithGithub = () => {
		const auth = getAuth(app);
		signInWithPopup(auth, new GithubAuthProvider());
	};

	const logout = async () => {
		const auth = getAuth(app);
		signOut(auth);
	};

	onMount(async () => {
		const auth = getAuth(app);
		onAuthStateChanged(auth, (newUser) => {
			console.log(user);
			user = newUser;
		});
	});
</script>

{#if user}
	<p>Signed in with {user.providerData[0].providerId}!</p>
	<button on:click={logout}>Logout</button>
{:else}
	<input type="email" id="email" placeholder="email" bind:value={email} />
	<input type="password" id="password" placeholder="password" bind:value={password} />
	<button on:click={login}>Login</button>
	<button on:click={loginWithGoogle}>Login with Google</button>
	<button on:click={loginWithGithub}>Login with Github</button>
{/if}
