<script lang="ts">
    import { invalidateAll } from "$app/navigation";
    import { useCreateSpaceUser, useDeleteSpaceUser } from "$lib/hooks";
    import { type Space, type SpaceUser, type User } from "@prisma/client";
    import { Plus, Trash } from "@steeze-ui/heroicons";
    import { Icon } from "@steeze-ui/svelte-icon";
    import { getContext } from "svelte";
    import Avatar from "./Avatar.svelte";

    export let space: Space;
    export let members: (SpaceUser & { user: User })[];

    let role = "USER";
    let email = "";
    let form: HTMLFormElement;

    const me = getContext<{ id: string }>("currentUser");
    const addMember = useCreateSpaceUser();
    const delMember = useDeleteSpaceUser();

    $: if ($addMember.isSuccess) {
        form.reset();
        invalidateAll();
    }

    $: if ($addMember.error) {
        alert(formatError($addMember.error));
    }

    $: if ($delMember.isSuccess) {
        invalidateAll();
    }

    $: if ($delMember.error) {
        alert(formatError($delMember.error));
    }

    async function onAddMember() {
        $addMember.mutate({
            data: {
                space: { connect: { id: space.id } },
                user: { connect: { email } },
                role,
            },
        });
    }

    function formatError(err: any) {
        if (err?.info?.code === "P2002") {
            return "User is already a member of this space";
        } else if (err?.info?.code === "P2025") {
            return "User not found";
        } else {
            return err?.info?.message ?? err?.message;
        }
    }

    async function onRemoveMember(id: string) {
        $delMember.mutate({ where: { id } });
    }
</script>

<div>
    <form class="flex flex-wrap gap-2 items-center mb-8 w-full" on:submit|preventDefault={onAddMember} bind:this={form}>
        <input
            type="email"
            bind:value={email}
            required
            placeholder="Type user email and enter to invite"
            class="input input-sm input-bordered flex-grow mr-2"
        />

        <select class="select select-sm mr-2" name="role" bind:value={role}>
            <option value={"USER"}>USER</option>
            <option value={"ADMIN"}>ADMIN</option>
        </select>

        <button type="submit">
            <Icon src={Plus} class="w-6 h-6 text-gray-500" />
        </button>
    </form>

    <ul class="space-y-2">
        {#each members as member (member.id)}
            <li class="flex flex-wrap w-full justify-between">
                <div class="flex items-center">
                    <div class="hidden md:block mr-2">
                        <Avatar size={32} email={member.user.email} />
                    </div>
                    <p class="w-36 md:w-48 line-clamp-1 mr-2">
                        {member.user.name || member.user.email}
                    </p>
                    <p>{member.role}</p>
                </div>
                <div class="flex items-center">
                    {#if me?.id !== member.user.id}
                        <button on:click={() => onRemoveMember(member.id)}
                            ><Icon src={Trash} class="w-4 h-4 text-gray-500" /></button
                        >
                    {/if}
                </div>
            </li>
        {/each}
    </ul>
</div>
