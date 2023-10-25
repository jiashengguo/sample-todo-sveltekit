<script lang="ts">
    import type { User } from '@prisma/client';
    import { getContext } from 'svelte';
    const currentUser = getContext<User>('currentUser');
    export let size: number = 32;
    export let email: string = currentUser.email;
</script>

<div class="tooltip" data-tip={email}>
    <img
        src={`https://picsum.photos/seed/${email}/200/300`}
        alt="avatar"
        class="rounded-full"
        style="width: {size}px; height: {size}px;"
    />
</div>
