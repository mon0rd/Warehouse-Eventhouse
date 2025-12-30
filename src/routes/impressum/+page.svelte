<script>
	import Header from '$lib/components/Header.svelte';
</script>

<section class="impressum">
	<Header />
	<h1>Impressum</h1>
	<h2>Sappho Studio GmbH</h2>
	<p>
		Hauptplatz 4 <br />
		3100 St. Pölten - Ratzersdorf
		<br />
		<br />
		<span>Telefon: <a href="tel:+436601067788">+43 660 106 77 88</a></span>
		<br />
		<span>E-Mail-Adresse: <a href="mailto:events@sapphostudio.at"> events@sapphostudio.at</a></span>
		<br />
		<br />
		Rechtsform: Gesellschaft mit beschränkter Haftung <br />
		Gewerberechtlicher GF: Frau Tetiana Kazynkina <br />
		UID-Nummer: ATU82749956 <br />
		Firmenbuchnummer: FN 665083 p
	</p>
</section>

<style lang="sass">
.impressum
    min-height: 98vh
    display: flex
    flex-direction: column
    justify-content: center
    h1
        font-size: 60px
    h2
        margin-top: 10px
    a
        color: inherit
</style>
