<script lang="ts">
	import { showErrorToast } from "$lib/api/error-toast";
	import {
		getPreferencesSnapshot,
		preferencesLoaded,
		setPreferences,
	} from "$lib/app-data/preferences.svelte";
	import SwitchField from "$lib/components/ui/switch-field/SwitchField.svelte";

	let pending = $state<boolean | null>(null);
	const value = $derived(pending ?? getPreferencesSnapshot().stayOnline);
</script>

<SwitchField
	title="البقاء متصلاً أثناء فتح التطبيق"
	description="تحديث حالة اتصالك في الخلفية تلقائياً أثناء استخدام التطبيق."
	disabled={!preferencesLoaded()}
	bind:checked={
		() => value,
		(newValue: boolean) => {
			pending = newValue;
			setPreferences({ stayOnline: newValue }).catch((error) => {
				pending = null;
				showErrorToast({ label: "فشل حفظ التفضيلات", error });
			});
		}
	}
/>
