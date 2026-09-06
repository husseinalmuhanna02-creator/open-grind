<script lang="ts">
	import { showErrorToast } from "$lib/api/error-toast";
	import {
		getPreferencesSnapshot,
		preferencesLoaded,
		setPreferences,
	} from "$lib/app-data/preferences.svelte";
	import SwitchField from "$lib/components/ui/switch-field/SwitchField.svelte";

	let pending = $state<boolean | null>(null);
	const value = $derived(
		pending ?? getPreferencesSnapshot().revealMessageRead,
	);
</script>

<SwitchField
	title="إظهار حالة قراءة الرسائل"
	description="السماح للآخرين بمعرفة ما إذا كنت قد قرأت رسائلهم. لن تتأثر إيصالات القراءة الخاصة بك."
	disabled={!preferencesLoaded()}
	bind:checked={
		() => value,
		(newValue: boolean) => {
			pending = newValue;
			setPreferences({ revealMessageRead: newValue }).catch((error) => {
				pending = null;
				showErrorToast({ label: "فشل حفظ التفضيلات", error });
			});
		}
	}
/>
