<script lang="ts">
	// Components
	import CallToAction from "$lib/components/layout/CallToAction.svelte";

	// Icons
	import StakeholderGrid from "$lib/components/layout/StakeholderGrid.svelte";
	import Story from "./Story.svelte";
	import SecondaryHero from "$lib/components/layout/hero-sections/SecondaryHero.svelte";
	import Culture from "$lib/components/layout/Culture.svelte";

	// Constants
	const values = [
		{
			title: "Collaboration moves ideas forward",
			description: "We work better when everyone's in the file—design, product, engineering, and beyond. Sharing early leads to better outcomes for all."
		},
		{
			title: "Creativity is made stronger together",
			description: "Great ideas deserve real-time input, rapid iteration, and room to evolve. Figma supports teams that think together and move fast."
		},
		{
			title: "Design belongs to everyone",
			description: "Access matters. Figma was built to be used by anyone—across roles, departments, and locations. It's your shared space for building better digital products."
		},
		{
			title: "Innovation is a team sport",
			description: "With seamless developer tools, AI to boost workflows, and the flexibility to scale, Figma is where cross-functional work shines."
		}
	];
</script>

<SecondaryHero
	title="Building great products, together bro"
	subtitle="Where design meets collaboration"
	imageSrc="/mockups/dark-screen-1750853573549.png"
/>

<!-- Story Section -->
<Story
	paragraphs={[
		"Figma has always been about making design more collaborative, but it started with a simple question: why do design and development often feel like different worlds? Teams passed files back and forth, feedback got lost in threads, and by the time engineering had what they needed, the design had already changed. It was slow, inefficient, and frustrating for everyone involved.",
		"So we built something different. A browser-based platform where anyone—designer, developer, product manager—could work in the same place, at the same time. No downloads, no silos, no chaos. Just real-time collaboration that feels like magic. You could design, prototype, share, present, even engineer handoff—all in one live file that never goes out of date.",
		"Today, product teams at some of the world's most innovative companies rely on Figma. Not just because it's powerful, but because it helps people work better together. It's clearer communication, faster execution, and fewer handoff headaches. We didn't just change how design happens—we closed the gap between idea and reality. And we're just getting started."
	]}
/>

<!-- Our Culture Section -->
<Culture {values} />

<!-- StakeholderGrid Section -->
<StakeholderGrid
	stakeholderType="team"
	title="Our team."
	subtitle="Leading design and development collaboration."
	stakeholders={[
		{
			name: "Diana Mounter",
			position: "Head of Design",
			bio: "Passionate about creating intuitive tools that empower designers and developers to work better together.",
			image: "/generated/image-a-professional-headshot-of-a-woman-busin.webp"
		},
		{
			name: "Levon Sharrow",
			position: "Director of Experience Design",
			bio: "Focused on building seamless experiences that bridge the gap between design vision and technical implementation.",
			image: "/generated/image-a-professional-headshot-of-a-man-executi.webp"
		}
	]}
/>

<!-- CTA Section -->
<CallToAction
	title="Ready to create together?"
	subtitle="Start designing, building, and collaborating—not later, now"
	description="Join millions of designers and developers who use Figma to collaborate in real-time, prototype faster, and ship products that bring ideas to life."
	imageSrc="/generated/image-a-group-of-professional-designers-engage.webp"
/>
