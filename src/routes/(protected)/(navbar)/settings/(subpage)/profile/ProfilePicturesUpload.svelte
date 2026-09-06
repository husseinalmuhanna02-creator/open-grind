<script lang="ts">
    import ProfilePictureSlot from "./ProfilePictureSlot.svelte";
    import { uploadProfilePhoto } from "$lib/api/users/profiles";

    const MAX_PHOTOS = 6;

    let { medias = $bindable() }: { medias: { mediaHash: string }[] } =
        $props();

    let fileInput: HTMLInputElement;
    let uploading = $state(false);

    const emptySlots = $derived(Math.max(0, MAX_PHOTOS - medias.length));

    function removePhoto(mediaHash: string) {
        medias = medias.filter((media) => media.mediaHash !== mediaHash);
    }

    function readFileAsBase64(file: File): Promise<string> {
        return new Promise((resolve, reject) => {
            const reader = new FileReader();
            reader.onload = () => {
                const result = reader.result as string;
                const base64 = result.split(',')[1];
                resolve(base64);
            };
            reader.onerror = (err) => reject(err);
            reader.readAsDataURL(file);
        });
    }

    async function handleFileSelect(event: Event) {
        const input = event.target as HTMLInputElement;
        if (!input.files || input.files.length === 0) return;

        const file = input.files[0];
        uploading = true;

        try {
            const base64Data = await readFileAsBase64(file);
            const res = await uploadProfilePhoto(base64Data, file.type || "image/jpeg");

            if (res && res.mediaHash) {
                medias = [...medias, { mediaHash: res.mediaHash }];
            }
        } catch (e) {
            console.error("Failed to upload photo:", e);
        } finally {
            uploading = false;
            input.value = "";
        }
    }
</script>

<input
    type="file"
    accept="image/*"
    class="hidden"
    bind:this={fileInput}
    onchange={handleFileSelect}
/>

<div class="grid grid-cols-3 gap-2">
    {#each medias as media, i (media.mediaHash + i)}
        <ProfilePictureSlot
            mediaHash={media.mediaHash}
            position={i + 1}
            onDelete={() => removePhoto(media.mediaHash)}
        />
    {/each}
    {#each Array.from({ length: emptySlots })}
        <button
            type="button"
            class="aspect-square rounded-xl border border-dashed border-border flex items-center justify-center cursor-pointer hover:bg-accent/50 disabled:opacity-50"
            disabled={uploading}
            onclick={() => fileInput?.click()}
        >
            {#if uploading}
                <span class="text-xs text-muted-foreground">جاري الرفع...</span>
            {:else}
                <span class="text-2xl text-muted-foreground">+</span>
            {/if}
        </button>
    {/each}
</div>
