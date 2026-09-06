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
		pending ?? getPreferencesSnapshot().revealProfileViews,
	);
</script>

<SwitchField
	title="إظهار مشاهدات الملف الشخصي"
	description="السماح للآخرين بمعرفة ما إذا كنت قد شاهدت ملفاتهم الشخصية. لن يتأثر سجل مشاهدات الملف الشخصي الخاص بك."
	disabled={!preferencesLoaded()}
	bind:checked={
		() => value,
		(newValue: boolean) => {
			pending = newValue;
			setPreferences({ revealProfileViews: newValue }).catch((error) => {
				pending = null;
				showErrorToast({ label: "فشل حفظ التفضيلات", error });
			});
		}
	}
/>
