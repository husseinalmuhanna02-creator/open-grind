<script lang="ts">
	import { page } from "$app/state";
	import { ArrowLeftIcon } from "phosphor-svelte";

	import ProgressiveBlur from "$lib/components/shared/ProgressiveBlur.svelte";

	const base = "/(protected)/(navbar)/settings/(subpage)";
	const routes: Record<string, { title: string; back: string }> = {
		[`${base}/account`]: { title: "إعدادات الحساب", back: "/settings" },
		[`${base}/account/privacy`]: {
			title: "الخصوصية",
			back: "/settings/account",
		},
		[`${base}/account/blocked`]: {
			title: "المستخدمون المحظورون",
			back: "/settings/account",
		},
		[`${base}/account/hidden`]: {
			title: "المستخدمون المخفيون",
			back: "/settings/account",
		},
		[`${base}/app`]: { title: "إعدادات التطبيق", back: "/settings" },
		[`${base}/profile`]: { title: "تعديل الملف الشخصي", back: "/settings" },
	};

	const current = $derived(
		(page.route.id && routes[page.route.id]) ?? {
			title: "",
			back: "/settings",
		},
	);
</script>

<ProgressiveBlur
	direction="topToBottom"
	class="fixed top-0 left-0 z-20 h-[calc(4.75rem+var(--safe-area-top))] w-full shrink-0"
	bgClass="bg-linear-to-b from-background to-transparent"
	contentClass="flex items-center h-full pe-5.5 pt-(--safe-area-top)"
	tag="nav"
>
	<a
		href={current.back}
		aria-label="رجوع"
		class="flex h-full w-19 shrink-0 items-center justify-center"
	>
		<ArrowLeftIcon size={32} />
	</a>
	<span class="min-w-0 truncate">
		{current.title}
	</span>
</ProgressiveBlur>
