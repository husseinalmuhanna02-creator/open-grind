<script lang="ts">
	import { CaretRightIcon } from "phosphor-svelte";
	import { toast } from "svelte-sonner";

	import ToastUnimplemented from "$lib/components/feedback/ToastUnimplemented.svelte";
	import * as Item from "$lib/components/ui/item";
	import { updatesSelfManaged } from "$lib/updates/capability.svelte";
	import AutomaticUpdatesSetting from "./AutomaticUpdatesSetting.svelte";
	import RevealMessageReadSetting from "./RevealMessageReadSetting.svelte";
	import RevealProfileViewSetting from "./RevealProfileViewSetting.svelte";
	import StayOnlineSetting from "./StayOnlineSetting.svelte";
	import UnitsSetting from "./UnitsSetting.svelte";
</script>

{#snippet item({
	title,
	unimplemented,
}: {
	title: string;
	unimplemented: { feature: string; issue: number };
})}
	<Item.Root variant="outline">
		{#snippet child({ props })}
			<a
				href="#/"
				{...props}
				onclick={(event) => {
					event.preventDefault();
					toast(ToastUnimplemented, {
						componentProps: unimplemented,
					});
				}}
			>
				<Item.Content class="max-cramped:min-w-0">
					<Item.Title
						class="inline-block max-w-full min-w-0 truncate text-start"
					>
						{title}
					</Item.Title>
				</Item.Content>
				<Item.Actions class="min-w-0">
					<CaretRightIcon class="size-4 shrink-0" />
				</Item.Actions>
			</a>
		{/snippet}
	</Item.Root>
{/snippet}
<h2>العرض</h2>
<UnitsSetting />
{@render item({
	title: "الإشعارات",
	unimplemented: { feature: "الإشعارات", issue: 45 },
})}
<h2>الخصوصية</h2>
<StayOnlineSetting />
<RevealMessageReadSetting />
<RevealProfileViewSetting />
<h2>الأمان</h2>
{@render item({
	title: "أيقونة التطبيق المتموهة",
	unimplemented: { feature: "أيقونة التطبيق المتموهة", issue: 97 },
})}
{@render item({ title: "رمز PIN", unimplemented: { feature: "رمز PIN", issue: 50 } })}
{#if updatesSelfManaged()}
	<h2>التحديثات</h2>
	<AutomaticUpdatesSetting />
{/if}

<style lang="postcss">
	@reference "$layout";

	h2 {
		@apply mt-2 truncate ps-4 text-xl font-semibold tracking-tight;
	}
</style>
