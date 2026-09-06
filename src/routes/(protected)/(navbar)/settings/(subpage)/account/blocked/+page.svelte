<script lang="ts" module>
	const scroll = { scrollY: 0 };
</script>

<script lang="ts">
	import { ProhibitIcon } from "phosphor-svelte";

	import {
		blockUser,
		getBlockedUsers,
		unblockUser,
	} from "$lib/api/browse/blocks";
	import ProfileList from "../profile-list/ProfileList.svelte";
</script>

<svelte:head>
	<title>المستخدمون المحظورون</title>
</svelte:head>

{#snippet icon(on: boolean)}
	<ProhibitIcon weight={on ? "fill" : "regular"} class="size-6" />
{/snippet}

<ProfileList
	loadIds={async () =>
		(await getBlockedUsers()).map(({ profileId }) => profileId)}
	setOn={({ profileId, on }) =>
		on ? blockUser({ profileId }) : unblockUser({ profileId })}
	{icon}
	{scroll}
	label="محظور"
	errorLabel={{
		turningOn: "فشل الحظر",
		turningOff: "فشل إلغاء الحظر",
	}}
	empty={{
		title: "لا يوجد مستخدمون محظورون",
		description: "سيظهر الأشخاص الذين تحظرهم هنا.",
	}}
/>
