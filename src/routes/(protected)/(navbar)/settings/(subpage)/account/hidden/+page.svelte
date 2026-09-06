<script lang="ts" module>
	const scroll = { scrollY: 0 };
</script>

<script lang="ts">
	import { EyeSlashIcon } from "phosphor-svelte";

	import {
		getHiddenUsers,
		hideUser,
		unhideUser,
	} from "$lib/api/browse/hides";
	import ProfileList from "../profile-list/ProfileList.svelte";
</script>

<svelte:head>
	<title>المستخدمون المخفيون</title>
</svelte:head>

{#snippet icon(on: boolean)}
	<EyeSlashIcon weight={on ? "fill" : "regular"} class="size-6" />
{/snippet}

<ProfileList
	eager
	loadIds={async () =>
		(await getHiddenUsers()).map(({ profileId }) => profileId)}
	setOn={({ profileId, on }) =>
		on ? hideUser({ profileId }) : unhideUser({ profileId })}
	{icon}
	{scroll}
	label="مخفي"
	errorLabel={{ turningOn: "فشل الإخفاء", turningOff: "فشل إلغاء الإخفاء" }}
	empty={{
		title: "لا يوجد مستخدمون مخفيون",
		description: "سيظهر الأشخاص الذين تخفيهم هنا.",
	}}
/>
