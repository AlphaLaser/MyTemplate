<script lang="ts">
	import { enhance } from '$app/forms';
	import { Button } from "$lib/components/ui/button/index.js";
	import * as Card from "$lib/components/ui/card/index.js";
	import { Input } from "$lib/components/ui/input/index.js";
	import { Label } from "$lib/components/ui/label/index.js";
	import type { ActionData, SubmitFunction } from './$types.js';

	export let form: ActionData;

	let loading = false;

	const handleSubmit: SubmitFunction = () => {
		loading = true;
		return async ({ update }) => {
			await update();
			loading = false;
		};
	};
</script>

<form method="POST" use:enhance={handleSubmit}>
	<Card.Root class="mx-auto max-w-sm">
		<Card.Header>
			<Card.Title class="text-2xl">Login</Card.Title>
			<Card.Description>
				Enter your email below to login to your account
			</Card.Description>
		</Card.Header>
		<Card.Content>
			{#if form?.message !== undefined}
				<div class="success {form?.success ? '' : 'fail'}">
					{form?.message}
				</div>
			{/if}
			<div class="grid gap-4">
				<div class="grid gap-2">
					<Label for="email">Email</Label>
					<Input
						id="email"
						name="email"
						type="email"
						placeholder="m@example.com"
						required
						value={form?.email ?? ''}
					/>
					{#if form?.errors?.email}
						<span class="flex items-center text-sm error">
							{form?.errors?.email}
						</span>
					{/if}
				</div>
				<Button type="submit" class="w-full">
					{loading ? 'Loading' : 'Login with Magic Link'}
				</Button>
			</div>
		</Card.Content>
	</Card.Root>
</form>
