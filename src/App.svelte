<!--suppress HtmlUnknownAttribute -->
<script>
  let promise;

  async function getRandomNumber() {
    const res = await fetch(`.netlify/functions/node-fetch/direction=n`);
    const text = await res.text();

    if (res.ok) {
      return text;
    } else {
      throw new Error(text);
    }
  }

  function handleClick() {
    promise = getRandomNumber();
  }

  function announcements(response) {
    return response ? JSON.parse(response).TrainAnnouncement : []
  }
</script>

<button on:click={handleClick}>northbound</button>

{#await promise}
  <p>...waiting</p>
{:then response}
  <p>There are {announcements(response).length} announcements</p>
  <ul>
    {#each announcements(response) as announcement}
      <li>{announcement.AdvertisedTrainIdent} {announcement.ActivityType} {announcement.LocationSignature}</li>
    {/each}
  </ul>
{:catch error}
  <p style="color: red">{error.message}</p>
{/await}
