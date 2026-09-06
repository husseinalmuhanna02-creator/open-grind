<script lang="ts">
	import { CaretRightIcon, SignOutIcon } from "phosphor-svelte";

	import { signOut } from "$lib/api/sign-out";
	import * as AlertDialog from "$lib/components/ui/alert-dialog";
	import * as Item from "$lib/components/ui/item";
	import ButtonItemContent from "./ButtonItemContent.svelte";

	let alertOpen = $state(false);
</script>

<Item.Root variant="outline">
	{#snippet child({ props })}
		<ButtonItemContent
			{...props}
			variant="outline"
			onclick={() => (alertOpen = true)}
		>
			<Item.Media>
				<SignOutIcon weight="fill" class="size-5" />
			</Item.Media>
			<Item.Content class="min-w-0">
				<Item.Title
					class="inline-block w-full min-w-0 truncate text-start"
				>
					تسجيل الخروج
				</Item.Title>
			</Item.Content>
			<Item.Actions>
				<CaretRightIcon class="size-4" />
			</Item.Actions>
		</ButtonItemContent>
	{/snippet}
</Item.Root>
<AlertDialog.Root bind:open={alertOpen}>
	<AlertDialog.Content>
		<AlertDialog.Header>
			<AlertDialog.Title>تسجيل الخروج؟</AlertDialog.Title>
			<AlertDialog.Description>
				هل أنت تأكد من أنك تريد تسجيل الخروج؟ يمكنك تسجيل الدخول مجدداً في أي وقت.
			</AlertDialog.Description>
		</AlertDialog.Header>
		<AlertDialog.Footer>
			<AlertDialog.Cancel size="lg">إلغاء</AlertDialog.Cancel>
			<AlertDialog.Action onclick={() => signOut()} size="lg">
				متابعة
			</AlertDialog.Action>
		</AlertDialog.Footer>
	</AlertDialog.Content>
</AlertDialog.Root>
