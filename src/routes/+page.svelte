<script lang="ts">
	// Components
	import UseCases from "$lib/components/layout/UseCases.svelte";
	import Summary from "$lib/components/layout/Summary.svelte";
	import Features from "$lib/components/layout/Features.svelte";
	import Testimonials from "$lib/components/layout/Testimonials.svelte";
	import CallToAction from "$lib/components/layout/CallToAction.svelte";
	import AboutTeaser from "$lib/components/layout/AboutTeaser.svelte";
	import LogoScroller from "$lib/components/layout/LogoScroller.svelte";
	import Hero from "$lib/components/layout/hero-sections/Hero.svelte";
</script>

<Hero
	title="Design, prototype, and build — together"
	subtitle="Figma brings your entire team into one collaborative workspace for faster feedback, cleaner handoff, and better products."
	imageSrc="/generated/image-a-diverse-design-team-collaborating-in-a.webp"
	centered={false}
/>
<LogoScroller 
	label="Trusted by industry leaders"
	logoUrls={[
		'https://logo.clearbit.com/github.com',
		'https://logo.clearbit.com/airbnb.com',
		'https://logo.clearbit.com/slack.com',
		'https://logo.clearbit.com/dropbox.com'
	]}
/>

<Summary
	title="What we do"
	text="Figma brings design, development, and product together in one collaborative workspace. From initial ideas to shipped products, teams work in real-time to create, prototype, and build digital experiences that matter."
/>

<Testimonials
	testimonials={[
		{
			name: "Keiran Holt",
			position: "VP of Product",
			company: "Tracerock",
			quote:
				"Figma helps us collaborate across functions and ships more things, faster. We cut our sprint cycles from 3 weeks to 10 days by eliminating design-dev miscommunication.\nOur velocity increased 40% once everyone could work in the same space.",
			image:
				"/generated/image-a-professional-man-in-a-modern-office-se.webp"
		},
		{
			name: "Laila Etienne",
			position: "Head of Product Design",
			company: "VergeLoop",
			quote:
				"We're finally designing and developing in sync. No more chasing down changes. This eliminated our biggest project risk—late-stage design revisions that used to cost us $50K per quarter.\nZero miscommunications means zero costly rebuilds.",
			image:
				"/generated/image-a-professional-woman-in-a-collaborative-.webp"
		},
		{
			name: "Brenna Walsh",
			position: "Product Director",
			company: "FlowState",
			quote:
				"Figma replaced six different tools and saved us $180K annually in licenses alone. Our team productivity jumped 35% when we consolidated everything into one platform.\nOnboarding new designers went from 2 weeks to 3 days.",
			image:
				"/generated/image-a-professional-woman-in-an-office-enviro.webp"
		}
	]}
/>

<Features
	title="Design, develop, and deliver—together"
	subtitle="Figma brings your entire product team into one shared space"
	features={[
		{
			title: "Work together in real time",
			description:
				"Design, comment, and iterate live in the same file—no saving, uploading, or waiting around. Everyone stays in sync, no matter where they sit."
		},
		{
			title: "Bring design and development closer",
			description:
				"With a dedicated developer workspace, design specs, assets, and code-ready details are easy to find and always up to date—no miscommunications or extra handoffs."
		},
		{
			title: "Use AI to speed up busywork",
			description:
				"Skip the blank page. Figma's AI features help you generate layouts, flows, mockups, and even starter code—so you can move from idea to insight, faster."
		},
		{
			title: "Continue work from anywhere",
			description:
				"No installs or hardware dependencies—open Figma from any browser, on any device, and pick up right where you left off."
		},
		{
			title: "Design systems that scale",
			description:
				"Reduce duplication and stay consistent across teams by connecting to reusable components, shared styles, and company-wide design systems."
		},
		{
			title: "Build and test interactive prototypes",
			description:
				"Visualize how a product should feel with fully interactive prototypes that you can present, share, and test—all without switching apps."
		}
	]}
/>

<UseCases
	title="Use Cases"
	subtitle="Real-world applications for your business needs"
	useCases={[
		{
			title: "Design cohesive user experiences",
			description: "Create consistent interfaces across products with shared design systems. Teams reduce design debt and ship features 3x faster when everyone works from the same components.",
			image: "/generated/image-a-group-of-ux-designers-gathered-around-.webp",
			link: {
				href: "/use-cases/design-systems",
				label: "Learn more"
			}
		},
		{
			title: "Test ideas before you build",
			description: "Validate concepts with interactive prototypes that feel real. Get stakeholder buy-in and user feedback early to avoid costly development changes later.",
			image: "/generated/image-a-product-team-in-a-modern-office-space-.webp",
			link: {
				href: "/use-cases/prototyping",
				label: "Explore prototyping"
			}
		},
		{
			title: "Bridge design and development",
			description: "Eliminate handoff friction with specs, assets, and code snippets that stay in sync. Developers get everything they need without hunting down designers.",
			image: "/generated/image-a-team-of-developers-working-together-in.webp",
			link: {
				href: "/use-cases/dev-handoff",
				label: "See dev features"
			}
		}
	]}
/>

<AboutTeaser />
<CallToAction
	title="Start faster. Ship smarter. Build together."
	subtitle="Try Figma free"
	description="The collaborative design platform built for teams to work faster—together. Create, prototype, and ship all in one place."
	imageSrc="/generated/image-a-cheerful-ux-designer-woman-with-a-brig.webp"
	callsToAction={[
		{
			href: "/get-started",
			label: "Get started for free",
			variant: "primary"
		},
		{
			href: "/demo",
			label: "Request demo",
			variant: "secondary"
		}
	]}
/>
