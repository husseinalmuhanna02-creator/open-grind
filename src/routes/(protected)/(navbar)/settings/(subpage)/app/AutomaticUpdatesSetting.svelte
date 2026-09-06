<script lang="ts">
	import { onMount } from "svelte";

	import { showErrorToast } from "$lib/api/error-toast";
	import SwitchField from "$lib/components/ui/switch-field/SwitchField.svelte";
	import { getUpdateSettings, setAutomaticUpdateChecks } from "$lib/updates";
	import { checkForUpdateNow } from "$lib/updates/updates-manager";

	let stored = $state<boolean | null>(null);
	let pending = $state<boolean | null>(null);
	const value = $derived(pending ?? stored ?? false);

	onMount(() => {
		getUpdateSettings()
			.then((settings) => {
				stored = settings.autoCheck;
			})
			.catch((error: unknown) => {
				showErrorToast({
					label: "تعذر قراءة إعدادات التحديثات",
					error,
				});
			});
	});
</script>

<SwitchField
	title="التحقق من التحديثات تلقائياً"
	description="طلب التحديثات بشكل دوري من git.opengrind.org. لا يتم إرسال أي معلومات شخصية، ولا يتم حفظ الطلبات أو تحليلها."
	disabled={stored === null}
	bind:checked={
		() => value,
		(newValue: boolean) => {
			pending = newValue;
			setAutomaticUpdateChecks(newValue)
				.then((settings) => {
					stored = settings.autoCheck;
					pending = null;
					if (settings.autoCheck) void checkForUpdateNow();
				})
				.catch((error: unknown) => {
					pending = null;
					showErrorToast({
						label: "تعذر حفظ إعدادات التحديثات",
						error,
					});
				});
		}
	}
/>
