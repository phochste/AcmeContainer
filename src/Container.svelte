<script lang="ts">
    import { getContainerList, type FileInfo } from './container';

    export let resource : string = "";

    let containerStack : string[] = [ ];
    let list : FileInfo[] = [];

    changeContainer({ url: resource });

    async function changeContainer(container : FileInfo) {
        if (container) {
            
            containerStack.push(resource);

            let next = await getContainerList(container.url);

            if (typeof(next) === 'undefined') {
                // Do nothing...
                containerStack.pop();
            }
            else {
                resource = container.url;
                list = next;
            }
        }
        else {
            let url  = containerStack.pop();
            let next = await getContainerList(url);

            if (typeof(next) === 'undefined') {
                // Do nothing...
            }
            else {
                resource = url;
                list = next;
            }
        }

        console.log(containerStack);
    }

</script>

<p> <b>{resource}</b> </p>

{#if list}
 <ul>
  {#if containerStack.length}
  <li>
        <div class="dir_open" on:click={() => changeContainer(null)}>..</div>
  </li>
  {/if}
  {#each list as item} 
    {#if item.isDir}
        <li><div class="dir" on:click={() => changeContainer(item)}>{item.name}</div></li>
    {:else}
       <li><div class="file">{item.name}</div></li>
    {/if}
  {/each}
 </ul>
{/if}

<style>
	div {
		padding: 0 0 0 1.5em;
		background: 0 0.1em no-repeat;
		background-size: 1em 1em;
	}

    .file {
        background-image: url(/images/file.svg);
    }

    .dir {
		background-image: url(/images/folder.svg);
        font-weight: bold;
	}

    .dir_open {
		background-image: url(/images/folder-open.svg);
        font-weight: bold;
    }

    ul {
		padding: 0.2em 0 0 0.5em;
		margin: 0 0 0 0.5em;
		list-style: none;
		border-left: 1px solid #eee;
	}

    li {
		padding: 0.2em 0;
	}
</style>
