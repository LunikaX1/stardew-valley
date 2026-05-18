<script lang="ts">
	type Season = 'spring' | 'summer' | 'fall' | 'winter';

	type Crop = {
		id: string;
		name: string;
		image: string;
		seasons: Season[];
		growDays: number;
		regrowDays?: number;
		yieldPerHarvest: number;
		seedPrice: number;
		sellPrice: number;
		color: string;
	};

	type Plan = {
		id: string;
		cropId: string;
		season: Season;
		plantDay: number;
		quantity: number;
	};

	type Harvest = {
		day: number;
		season: Season;
		absoluteDay: number;
		items: number;
		gold: number;
	};

	type CalendarEvent = {
		id: string;
		day: number;
		title: string;
		type: 'birthday' | 'festival' | 'shop' | 'season' | 'special' | 'bookseller';
		icon?: string;
	};

	const cropImages = import.meta.glob(
		'../lib/assets/Crops/{Amaranth,Artichoke,Beet,Blueberry,Blue_Jazz,Bok_Choy,Broccoli,Carrot,Cauliflower,Coffee_Bean,Corn,Cranberries,Eggplant,Fairy_Rose,Garlic,Grape,Green_Bean,Hops,Hot_Pepper,Kale,Melon,Parsnip,Poppy,Potato,Powdermelon,Pumpkin,Radish,Red_Cabbage,Rhubarb,Starfruit,Strawberry,Summer_Spangle,Summer_Squash,Sunflower,Tomato,Tulip,Unmilled_Rice,Wheat,Yam}.png',
		{
			eager: true,
			import: 'default',
			query: '?url'
		}
	) as Record<string, string>;

	function cropImage(fileName: string) {
		return cropImages[`../lib/assets/Crops/${fileName}.png`];
	}

	const eventImages = import.meta.glob(
		'../lib/assets/Crops/{24px-Iridium_Quality_Icon,18px-Gold}.png',
		{
			eager: true,
			import: 'default',
			query: '?url'
		}
	) as Record<string, string>;

	function eventImage(fileName: string) {
		return eventImages[`../lib/assets/Crops/${fileName}.png`];
	}

	const villagerImages = import.meta.glob('../lib/assets/Villagers/*.png', {
		eager: true,
		import: 'default',
		query: '?url'
	}) as Record<string, string>;

	function villagerImage(fileName: string) {
		return villagerImages[`../lib/assets/Villagers/${fileName}.png`];
	}

	function birthdayEvent(id: string, day: number, villager: string): CalendarEvent {
		return {
			id,
			day,
			title: `${villager} Birthday`,
			type: 'birthday',
			icon: villagerImage(villager)
		};
	}

	const eventTypeLabels: Record<CalendarEvent['type'], string> = {
		festival: 'Festivals',
		birthday: 'Birthdays',
		shop: 'Shop',
		season: 'Seasonal',
		special: 'Special',
		bookseller: 'Bookseller'
	};

	const seasons: Record<
		Season,
		{
			name: string;
			accent: string;
			deep: string;
		}
	> = {
		spring: { name: 'Spring', accent: '#78c850', deep: '#3f8f38' },
		summer: { name: 'Summer', accent: '#f7b232', deep: '#c56b24' },
		fall: { name: 'Fall', accent: '#d96b2b', deep: '#923f22' },
		winter: { name: 'Winter', accent: '#75b6df', deep: '#356f9b' }
	};

	// Crop stats are based on the local Stardew Valley Wiki crops HTML in the project root.
	const crops: Crop[] = [
		{
			id: 'blue-jazz',
			name: 'Blue Jazz',
			image: cropImage('Blue_Jazz'),
			seasons: ['spring'],
			growDays: 7,
			yieldPerHarvest: 1,
			seedPrice: 30,
			sellPrice: 50,
			color: '#7f91df'
		},
		{
			id: 'carrot',
			name: 'Carrot',
			image: cropImage('Carrot'),
			seasons: ['spring'],
			growDays: 3,
			yieldPerHarvest: 1,
			seedPrice: 0,
			sellPrice: 35,
			color: '#f28b35'
		},
		{
			id: 'cauliflower',
			name: 'Cauliflower',
			image: cropImage('Cauliflower'),
			seasons: ['spring'],
			growDays: 12,
			yieldPerHarvest: 1,
			seedPrice: 80,
			sellPrice: 175,
			color: '#e8ead7'
		},
		{
			id: 'coffee-bean',
			name: 'Coffee Bean',
			image: cropImage('Coffee_Bean'),
			seasons: ['spring', 'summer'],
			growDays: 10,
			regrowDays: 2,
			yieldPerHarvest: 4,
			seedPrice: 2500,
			sellPrice: 15,
			color: '#8f5a35'
		},
		{
			id: 'garlic',
			name: 'Garlic',
			image: cropImage('Garlic'),
			seasons: ['spring'],
			growDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 40,
			sellPrice: 60,
			color: '#f2e6c8'
		},
		{
			id: 'green-bean',
			name: 'Green Bean',
			image: cropImage('Green_Bean'),
			seasons: ['spring'],
			growDays: 10,
			regrowDays: 3,
			yieldPerHarvest: 1,
			seedPrice: 60,
			sellPrice: 40,
			color: '#65a85b'
		},
		{
			id: 'kale',
			name: 'Kale',
			image: cropImage('Kale'),
			seasons: ['spring'],
			growDays: 6,
			yieldPerHarvest: 1,
			seedPrice: 70,
			sellPrice: 110,
			color: '#4f9b52'
		},
		{
			id: 'parsnip',
			name: 'Parsnip',
			image: cropImage('Parsnip'),
			seasons: ['spring'],
			growDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 20,
			sellPrice: 35,
			color: '#f5d76e'
		},
		{
			id: 'potato',
			name: 'Potato',
			image: cropImage('Potato'),
			seasons: ['spring'],
			growDays: 6,
			yieldPerHarvest: 1.25,
			seedPrice: 50,
			sellPrice: 80,
			color: '#c99a55'
		},
		{
			id: 'rhubarb',
			name: 'Rhubarb',
			image: cropImage('Rhubarb'),
			seasons: ['spring'],
			growDays: 13,
			yieldPerHarvest: 1,
			seedPrice: 100,
			sellPrice: 220,
			color: '#d15f67'
		},
		{
			id: 'strawberry',
			name: 'Strawberry',
			image: cropImage('Strawberry'),
			seasons: ['spring'],
			growDays: 8,
			regrowDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 100,
			sellPrice: 120,
			color: '#e85c55'
		},
		{
			id: 'tulip',
			name: 'Tulip',
			image: cropImage('Tulip'),
			seasons: ['spring'],
			growDays: 6,
			yieldPerHarvest: 1,
			seedPrice: 20,
			sellPrice: 30,
			color: '#d76bb3'
		},
		{
			id: 'unmilled-rice',
			name: 'Unmilled Rice',
			image: cropImage('Unmilled_Rice'),
			seasons: ['spring'],
			growDays: 8,
			yieldPerHarvest: 1,
			seedPrice: 40,
			sellPrice: 30,
			color: '#c7d987'
		},
		{
			id: 'blueberry',
			name: 'Blueberry',
			image: cropImage('Blueberry'),
			seasons: ['summer'],
			growDays: 13,
			regrowDays: 4,
			yieldPerHarvest: 3,
			seedPrice: 80,
			sellPrice: 50,
			color: '#576bd6'
		},
		{
			id: 'corn',
			name: 'Corn',
			image: cropImage('Corn'),
			seasons: ['summer', 'fall'],
			growDays: 14,
			regrowDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 150,
			sellPrice: 50,
			color: '#f2ca4c'
		},
		{
			id: 'hops',
			name: 'Hops',
			image: cropImage('Hops'),
			seasons: ['summer'],
			growDays: 11,
			regrowDays: 1,
			yieldPerHarvest: 1,
			seedPrice: 60,
			sellPrice: 25,
			color: '#87b54e'
		},
		{
			id: 'hot-pepper',
			name: 'Hot Pepper',
			image: cropImage('Hot_Pepper'),
			seasons: ['summer'],
			growDays: 5,
			regrowDays: 3,
			yieldPerHarvest: 1,
			seedPrice: 40,
			sellPrice: 40,
			color: '#d93d32'
		},
		{
			id: 'melon',
			name: 'Melon',
			image: cropImage('Melon'),
			seasons: ['summer'],
			growDays: 12,
			yieldPerHarvest: 1,
			seedPrice: 80,
			sellPrice: 250,
			color: '#e98682'
		},
		{
			id: 'poppy',
			name: 'Poppy',
			image: cropImage('Poppy'),
			seasons: ['summer'],
			growDays: 7,
			yieldPerHarvest: 1,
			seedPrice: 100,
			sellPrice: 140,
			color: '#e5533f'
		},
		{
			id: 'radish',
			name: 'Radish',
			image: cropImage('Radish'),
			seasons: ['summer'],
			growDays: 6,
			yieldPerHarvest: 1,
			seedPrice: 40,
			sellPrice: 90,
			color: '#d85b72'
		},
		{
			id: 'red-cabbage',
			name: 'Red Cabbage',
			image: cropImage('Red_Cabbage'),
			seasons: ['summer'],
			growDays: 9,
			yieldPerHarvest: 1,
			seedPrice: 100,
			sellPrice: 260,
			color: '#8f4f9e'
		},
		{
			id: 'starfruit',
			name: 'Starfruit',
			image: cropImage('Starfruit'),
			seasons: ['summer'],
			growDays: 13,
			yieldPerHarvest: 1,
			seedPrice: 400,
			sellPrice: 750,
			color: '#f4cf45'
		},
		{
			id: 'summer-spangle',
			name: 'Summer Spangle',
			image: cropImage('Summer_Spangle'),
			seasons: ['summer'],
			growDays: 8,
			yieldPerHarvest: 1,
			seedPrice: 50,
			sellPrice: 90,
			color: '#e9aa3f'
		},
		{
			id: 'summer-squash',
			name: 'Summer Squash',
			image: cropImage('Summer_Squash'),
			seasons: ['summer'],
			growDays: 6,
			regrowDays: 3,
			yieldPerHarvest: 1,
			seedPrice: 10,
			sellPrice: 45,
			color: '#f4cd55'
		},
		{
			id: 'sunflower',
			name: 'Sunflower',
			image: cropImage('Sunflower'),
			seasons: ['summer', 'fall'],
			growDays: 8,
			yieldPerHarvest: 1,
			seedPrice: 200,
			sellPrice: 80,
			color: '#e7b83a'
		},
		{
			id: 'tomato',
			name: 'Tomato',
			image: cropImage('Tomato'),
			seasons: ['summer'],
			growDays: 11,
			regrowDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 50,
			sellPrice: 60,
			color: '#d95132'
		},
		{
			id: 'wheat',
			name: 'Wheat',
			image: cropImage('Wheat'),
			seasons: ['summer', 'fall'],
			growDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 10,
			sellPrice: 25,
			color: '#d7b85a'
		},
		{
			id: 'amaranth',
			name: 'Amaranth',
			image: cropImage('Amaranth'),
			seasons: ['fall'],
			growDays: 7,
			yieldPerHarvest: 1,
			seedPrice: 70,
			sellPrice: 150,
			color: '#b55a8d'
		},
		{
			id: 'artichoke',
			name: 'Artichoke',
			image: cropImage('Artichoke'),
			seasons: ['fall'],
			growDays: 8,
			yieldPerHarvest: 1,
			seedPrice: 30,
			sellPrice: 160,
			color: '#75a85e'
		},
		{
			id: 'beet',
			name: 'Beet',
			image: cropImage('Beet'),
			seasons: ['fall'],
			growDays: 6,
			yieldPerHarvest: 1,
			seedPrice: 20,
			sellPrice: 100,
			color: '#b9415d'
		},
		{
			id: 'bok-choy',
			name: 'Bok Choy',
			image: cropImage('Bok_Choy'),
			seasons: ['fall'],
			growDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 50,
			sellPrice: 80,
			color: '#8fcf6a'
		},
		{
			id: 'broccoli',
			name: 'Broccoli',
			image: cropImage('Broccoli'),
			seasons: ['fall'],
			growDays: 8,
			regrowDays: 4,
			yieldPerHarvest: 1,
			seedPrice: 5,
			sellPrice: 70,
			color: '#4f9a4c'
		},
		{
			id: 'cranberries',
			name: 'Cranberries',
			image: cropImage('Cranberries'),
			seasons: ['fall'],
			growDays: 7,
			regrowDays: 5,
			yieldPerHarvest: 2,
			seedPrice: 240,
			sellPrice: 75,
			color: '#b92d48'
		},
		{
			id: 'eggplant',
			name: 'Eggplant',
			image: cropImage('Eggplant'),
			seasons: ['fall'],
			growDays: 5,
			regrowDays: 5,
			yieldPerHarvest: 1,
			seedPrice: 20,
			sellPrice: 60,
			color: '#7651a8'
		},
		{
			id: 'fairy-rose',
			name: 'Fairy Rose',
			image: cropImage('Fairy_Rose'),
			seasons: ['fall'],
			growDays: 12,
			yieldPerHarvest: 1,
			seedPrice: 200,
			sellPrice: 290,
			color: '#e58fc0'
		},
		{
			id: 'grape',
			name: 'Grape',
			image: cropImage('Grape'),
			seasons: ['fall'],
			growDays: 10,
			regrowDays: 3,
			yieldPerHarvest: 1,
			seedPrice: 60,
			sellPrice: 80,
			color: '#7857b8'
		},
		{
			id: 'pumpkin',
			name: 'Pumpkin',
			image: cropImage('Pumpkin'),
			seasons: ['fall'],
			growDays: 13,
			yieldPerHarvest: 1,
			seedPrice: 100,
			sellPrice: 320,
			color: '#f08a27'
		},
		{
			id: 'yam',
			name: 'Yam',
			image: cropImage('Yam'),
			seasons: ['fall'],
			growDays: 10,
			yieldPerHarvest: 1,
			seedPrice: 60,
			sellPrice: 160,
			color: '#b86436'
		},
		{
			id: 'powdermelon',
			name: 'Powdermelon',
			image: cropImage('Powdermelon'),
			seasons: ['winter'],
			growDays: 7,
			yieldPerHarvest: 1,
			seedPrice: 20,
			sellPrice: 60,
			color: '#acd7ef'
		}
	];

	const weekDays = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
	const days = Array.from({ length: 28 }, (_, index) => index + 1);
	const seasonOrder: Season[] = ['spring', 'summer', 'fall', 'winter'];
	const springEvents: CalendarEvent[] = [
		{ id: 'spring-egg-festival', day: 13, title: 'Egg Festival', type: 'festival' },
		{
			id: 'spring-desert-festival-15',
			day: 15,
			title: 'Desert Festival',
			type: 'festival',
			icon: eventImage('24px-Iridium_Quality_Icon')
		},
		{
			id: 'spring-desert-festival-16',
			day: 16,
			title: 'Desert Festival',
			type: 'festival',
			icon: eventImage('24px-Iridium_Quality_Icon')
		},
		{
			id: 'spring-desert-festival-17',
			day: 17,
			title: 'Desert Festival',
			type: 'festival',
			icon: eventImage('24px-Iridium_Quality_Icon')
		},
		{ id: 'spring-flower-dance', day: 24, title: 'Flower Dance', type: 'festival' },
		{ id: 'spring-salmonberry-15', day: 15, title: 'Salmonberry Season', type: 'season' },
		{ id: 'spring-salmonberry-16', day: 16, title: 'Salmonberry Season', type: 'season' },
		{ id: 'spring-salmonberry-17', day: 17, title: 'Salmonberry Season', type: 'season' },
		{ id: 'spring-salmonberry-18', day: 18, title: 'Salmonberry Season', type: 'season' },
		{
			id: 'spring-pot-of-gold',
			day: 17,
			title: 'Pot of Gold',
			type: 'special',
			icon: eventImage('18px-Gold')
		},
		{ id: 'spring-bookseller-11', day: 11, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'spring-bookseller-12', day: 12, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'spring-bookseller-21', day: 21, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'spring-bookseller-22', day: 22, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'spring-bookseller-25', day: 25, title: 'Bookseller possible', type: 'bookseller' },
		birthdayEvent('spring-kent-birthday', 4, 'Kent'),
		birthdayEvent('spring-lewis-birthday', 7, 'Lewis'),
		birthdayEvent('spring-vincent-birthday', 10, 'Vincent'),
		birthdayEvent('spring-haley-birthday', 14, 'Haley'),
		birthdayEvent('spring-pam-birthday', 18, 'Pam'),
		birthdayEvent('spring-shane-birthday', 20, 'Shane'),
		birthdayEvent('spring-pierre-birthday', 26, 'Pierre'),
		birthdayEvent('spring-emily-birthday', 27, 'Emily')
	];

	const summerEvents: CalendarEvent[] = [
		{ id: 'summer-luau', day: 11, title: 'Luau', type: 'festival' },
		{ id: 'summer-trout-derby-20', day: 20, title: 'Trout Derby', type: 'festival' },
		{ id: 'summer-trout-derby-21', day: 21, title: 'Trout Derby', type: 'festival' },
		{ id: 'summer-jellies', day: 28, title: 'Moonlight Jellies', type: 'festival' },
		{ id: 'summer-beach-forage-12', day: 12, title: 'Beach forageables', type: 'season' },
		{ id: 'summer-beach-forage-13', day: 13, title: 'Beach forageables', type: 'season' },
		{ id: 'summer-beach-forage-14', day: 14, title: 'Beach forageables', type: 'season' },
		{ id: 'summer-bookseller-9', day: 9, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'summer-bookseller-12', day: 12, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'summer-bookseller-18', day: 18, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'summer-bookseller-25', day: 25, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'summer-bookseller-27', day: 27, title: 'Bookseller possible', type: 'bookseller' },
		birthdayEvent('summer-jas-birthday', 4, 'Jas'),
		birthdayEvent('summer-gus-birthday', 8, 'Gus'),
		birthdayEvent('summer-maru-birthday', 10, 'Maru'),
		birthdayEvent('summer-alex-birthday', 13, 'Alex'),
		birthdayEvent('summer-sam-birthday', 17, 'Sam'),
		birthdayEvent('summer-demetrius-birthday', 19, 'Demetrius'),
		birthdayEvent('summer-dwarf-birthday', 22, 'Dwarf'),
		birthdayEvent('summer-willy-birthday', 24, 'Willy'),
		birthdayEvent('summer-leo-birthday', 26, 'Leo')
	];

	const fallEvents: CalendarEvent[] = [
		{ id: 'fall-fair', day: 16, title: 'Stardew Valley Fair', type: 'festival' },
		{ id: 'fall-spirits-eve', day: 27, title: "Spirit's Eve", type: 'festival' },
		{ id: 'fall-blackberry-8', day: 8, title: 'Blackberry Season', type: 'season' },
		{ id: 'fall-blackberry-9', day: 9, title: 'Blackberry Season', type: 'season' },
		{ id: 'fall-blackberry-10', day: 10, title: 'Blackberry Season', type: 'season' },
		{ id: 'fall-blackberry-11', day: 11, title: 'Blackberry Season', type: 'season' },
		{ id: 'fall-bookseller-4', day: 4, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-7', day: 7, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-8', day: 8, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-9', day: 9, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-12', day: 12, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-19', day: 19, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-22', day: 22, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'fall-bookseller-25', day: 25, title: 'Bookseller possible', type: 'bookseller' },
		birthdayEvent('fall-penny-birthday', 2, 'Penny'),
		birthdayEvent('fall-elliott-birthday', 5, 'Elliott'),
		birthdayEvent('fall-jodi-birthday', 11, 'Jodi'),
		birthdayEvent('fall-abigail-birthday', 13, 'Abigail'),
		birthdayEvent('fall-sandy-birthday', 15, 'Sandy'),
		birthdayEvent('fall-marnie-birthday', 18, 'Marnie'),
		birthdayEvent('fall-robin-birthday', 21, 'Robin'),
		birthdayEvent('fall-george-birthday', 24, 'George')
	];

	const winterEvents: CalendarEvent[] = [
		{ id: 'winter-ice-festival', day: 8, title: 'Festival of Ice', type: 'festival' },
		{ id: 'winter-squidfest-12', day: 12, title: 'SquidFest', type: 'festival' },
		{ id: 'winter-squidfest-13', day: 13, title: 'SquidFest', type: 'festival' },
		{ id: 'winter-night-market-15', day: 15, title: 'Night Market', type: 'festival', icon: eventImage('24px-Iridium_Quality_Icon') },
		{ id: 'winter-night-market-16', day: 16, title: 'Night Market', type: 'festival', icon: eventImage('24px-Iridium_Quality_Icon') },
		{ id: 'winter-night-market-17', day: 17, title: 'Night Market', type: 'festival', icon: eventImage('24px-Iridium_Quality_Icon') },
		{ id: 'winter-star-feast', day: 25, title: 'Feast of the Winter Star', type: 'festival' },
		{ id: 'winter-bookseller-5', day: 5, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'winter-bookseller-11', day: 11, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'winter-bookseller-12', day: 12, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'winter-bookseller-19', day: 19, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'winter-bookseller-22', day: 22, title: 'Bookseller possible', type: 'bookseller' },
		{ id: 'winter-bookseller-24', day: 24, title: 'Bookseller possible', type: 'bookseller' },
		birthdayEvent('winter-krobus-birthday', 1, 'Krobus'),
		birthdayEvent('winter-linus-birthday', 3, 'Linus'),
		birthdayEvent('winter-caroline-birthday', 7, 'Caroline'),
		birthdayEvent('winter-sebastian-birthday', 10, 'Sebastian'),
		birthdayEvent('winter-harvey-birthday', 14, 'Harvey'),
		birthdayEvent('winter-wizard-birthday', 17, 'Wizard'),
		birthdayEvent('winter-evelyn-birthday', 20, 'Evelyn'),
		birthdayEvent('winter-leah-birthday', 23, 'Leah'),
		birthdayEvent('winter-clint-birthday', 26, 'Clint')
	];

	const calendarEventsBySeason: Record<Season, CalendarEvent[]> = {
		spring: springEvents,
		summer: summerEvents,
		fall: fallEvents,
		winter: winterEvents
	};

	let selectedSeason = $state<Season>('spring');
	let selectedCropId = $state('parsnip');
	let plantQuantity = $state(24);
	let draggedPlanId = $state<string | null>(null);
	let draggingCropId = $state<string | null>(null);
	let dragOverDay = $state<number | null>(null);
	let modalPlanId = $state<string | null>(null);
	let eventFilters = $state<Record<CalendarEvent['type'], boolean>>({
		festival: true,
		birthday: true,
		shop: true,
		season: true,
		special: true,
		bookseller: true
	});
	function loadPlans(): Plan[] {
		if (typeof localStorage === 'undefined') return [];
		try {
			return JSON.parse(localStorage.getItem('sdv-plans') ?? '[]');
		} catch {
			return [];
		}
	}

	let plans = $state<Plan[]>(loadPlans());

	$effect(() => {
		localStorage.setItem('sdv-plans', JSON.stringify(plans));
	});

	const season = $derived(seasons[selectedSeason]);
	const seasonCrops = $derived(crops.filter((crop) => crop.seasons.includes(selectedSeason)));
	const selectedCrop = $derived(crops.find((crop) => crop.id === selectedCropId) ?? seasonCrops[0]);
	const visiblePlans = $derived(
		plans.filter(
			(plan) =>
				plan.season === selectedSeason ||
				getHarvests(plan).some((harvest) => harvest.season === selectedSeason)
		)
	);
	const totals = $derived(calculateTotals(visiblePlans));

	const modalPlan = $derived(plans.find((p) => p.id === modalPlanId) ?? null);
	const modalCrop = $derived(modalPlan ? getCrop(modalPlan.cropId) : null);
	const modalHarvests = $derived(modalPlan ? getHarvests(modalPlan) : []);

	const dragPreviewHarvestDays = $derived.by(() => {
		if (dragOverDay === null) return [] as number[];
		let cropId: string | null = draggingCropId;
		if (!cropId && draggedPlanId) {
			cropId = plans.find((p) => p.id === draggedPlanId)?.cropId ?? null;
		}
		if (!cropId) return [] as number[];
		const crop = getCrop(cropId);
		if (!crop.seasons.includes(selectedSeason)) return [] as number[];
		const result: number[] = [];
		let day = absoluteDay(selectedSeason, dragOverDay) + crop.growDays;
		while (day <= 112) {
			const harvestSeason = seasonFromAbsoluteDay(day);
			if (!harvestSeason || !crop.seasons.includes(harvestSeason)) break;
			if (harvestSeason === selectedSeason) {
				result.push(dayInSeasonFromAbsoluteDay(day));
			}
			if (!crop.regrowDays) break;
			day += crop.regrowDays;
		}
		return result;
	});

	function getCrop(cropId: string) {
		return crops.find((crop) => crop.id === cropId) ?? crops[0];
	}

	function absoluteDay(seasonName: Season, day: number) {
		return seasonOrder.indexOf(seasonName) * 28 + day;
	}

	function seasonFromAbsoluteDay(day: number): Season | null {
		return seasonOrder[Math.floor((day - 1) / 28)] ?? null;
	}

	function dayInSeasonFromAbsoluteDay(day: number) {
		return ((day - 1) % 28) + 1;
	}

	function getHarvests(plan: Plan): Harvest[] {
		const crop = getCrop(plan.cropId);
		const harvests: Harvest[] = [];
		let harvestDay = absoluteDay(plan.season, plan.plantDay) + crop.growDays;

		while (harvestDay <= 112) {
			const harvestSeason = seasonFromAbsoluteDay(harvestDay);
			if (!harvestSeason || !crop.seasons.includes(harvestSeason)) {
				break;
			}

			const items = Math.round(plan.quantity * crop.yieldPerHarvest);

			harvests.push({
				day: dayInSeasonFromAbsoluteDay(harvestDay),
				season: harvestSeason,
				absoluteDay: harvestDay,
				items,
				gold: items * crop.sellPrice
			});

			if (!crop.regrowDays) {
				break;
			}

			harvestDay += crop.regrowDays;
		}

		return harvests;
	}

	function calculateTotals(currentPlans: Plan[]) {
		return currentPlans.reduce(
			(total, plan) => {
				const crop = getCrop(plan.cropId);
				const harvests = getHarvests(plan).filter((harvest) => harvest.season === selectedSeason);
				const items = harvests.reduce((sum, harvest) => sum + harvest.items, 0);
				const revenue = harvests.reduce((sum, harvest) => sum + harvest.gold, 0);
				const seedCost = plan.season === selectedSeason ? plan.quantity * crop.seedPrice : 0;

				return {
					items: total.items + items,
					revenue: total.revenue + revenue,
					seedCost: total.seedCost + seedCost,
					profit: total.profit + revenue - seedCost
				};
			},
			{ items: 0, revenue: 0, seedCost: 0, profit: 0 }
		);
	}

	function getPlanSummary(plan: Plan) {
		const crop = getCrop(plan.cropId);
		const harvests = getHarvests(plan).filter((harvest) => harvest.season === selectedSeason);
		const items = harvests.reduce((sum, harvest) => sum + harvest.items, 0);
		const revenue = harvests.reduce((sum, harvest) => sum + harvest.gold, 0);
		const seedCost = plan.season === selectedSeason ? plan.quantity * crop.seedPrice : 0;

		return {
			harvestDays: harvests.map((harvest) => harvest.day),
			items,
			revenue,
			seedCost,
			profit: revenue - seedCost
		};
	}

	function plansForDay(day: number) {
		return visiblePlans.filter((plan) => plan.season === selectedSeason && plan.plantDay === day);
	}

	function harvestsForDay(day: number) {
		return visiblePlans.flatMap((plan) =>
			getHarvests(plan)
				.filter((harvest) => harvest.season === selectedSeason && harvest.day === day)
				.map((harvest) => ({ ...harvest, plan, crop: getCrop(plan.cropId) }))
		);
	}

	function eventsForDay(day: number) {
		const events: CalendarEvent[] = [];

		if (day % 7 === 3) {
			events.push({
				id: `${selectedSeason}-pierre-closed-${day}`,
				day,
				title: "Pierre's closed",
				type: 'shop',
				icon: eventImage('24px-Pierre_Icon')
			});
		}

		events.push(...calendarEventsBySeason[selectedSeason].filter((event) => event.day === day));

		return events.filter((event) => eventFilters[event.type]);
	}

	function toggleEventFilter(type: CalendarEvent['type']) {
		eventFilters = { ...eventFilters, [type]: !eventFilters[type] };
	}

	function selectSeason(nextSeason: Season) {
		selectedSeason = nextSeason;
		selectedCropId = crops.find((crop) => crop.seasons.includes(nextSeason))?.id ?? crops[0].id;
	}

	function addPlan(cropId: string, day: number) {
		const crop = getCrop(cropId);

		if (!crop.seasons.includes(selectedSeason)) {
			return;
		}

		plans = [
			...plans,
			{
				id: `plan-${crypto.randomUUID()}`,
				cropId,
				season: selectedSeason,
				plantDay: day,
				quantity: Math.max(1, Math.round(plantQuantity))
			}
		];
	}

	function movePlan(planId: string, day: number) {
		plans = plans.map((plan) => (plan.id === planId ? { ...plan, plantDay: day } : plan));
	}

	function updatePlanQuantity(planId: string, quantity: number) {
		plans = plans.map((plan) =>
			plan.id === planId ? { ...plan, quantity: Math.max(1, Math.round(quantity || 1)) } : plan
		);
	}

	function removePlan(planId: string) {
		plans = plans.filter((plan) => plan.id !== planId);
	}

	function getDragPreview(day: number): { crop: Crop; harvestLabels: string[] } | null {
		let cropId: string | null = null;

		if (draggingCropId) {
			cropId = draggingCropId;
		} else if (draggedPlanId) {
			cropId = plans.find((p) => p.id === draggedPlanId)?.cropId ?? null;
		}

		if (!cropId) return null;

		const crop = getCrop(cropId);
		if (!crop.seasons.includes(selectedSeason)) return null;

		const harvestLabels: string[] = [];
		let harvestDay = absoluteDay(selectedSeason, day) + crop.growDays;
		while (harvestDay <= 112) {
			const harvestSeason = seasonFromAbsoluteDay(harvestDay);
			if (!harvestSeason || !crop.seasons.includes(harvestSeason)) break;
			harvestLabels.push(`${seasons[harvestSeason].name} ${dayInSeasonFromAbsoluteDay(harvestDay)}`);
			if (!crop.regrowDays) break;
			harvestDay += crop.regrowDays;
		}

		return { crop, harvestLabels };
	}

	function onCropDragStart(event: DragEvent, cropId: string) {
		draggingCropId = cropId;
		draggedPlanId = null;
		event.dataTransfer?.setData('text/plain', `crop:${cropId}`);
		event.dataTransfer?.setData('application/x-crop-id', cropId);
	}

	function onPlanDragStart(event: DragEvent, planId: string) {
		draggedPlanId = planId;
		draggingCropId = null;
		event.dataTransfer?.setData('text/plain', `plan:${planId}`);
		event.dataTransfer?.setData('application/x-plan-id', planId);
	}

	function onDrop(event: DragEvent, day: number) {
		event.preventDefault();
		dragOverDay = null;

		const planId = event.dataTransfer?.getData('application/x-plan-id') || draggedPlanId;
		const cropId = event.dataTransfer?.getData('application/x-crop-id');

		if (planId) {
			movePlan(planId, day);
		} else if (cropId) {
			addPlan(cropId, day);
		}

		draggedPlanId = null;
		draggingCropId = null;
	}

	function onDragOver(event: DragEvent, day: number) {
		event.preventDefault();
		dragOverDay = day;
	}

	function onDragEnd() {
		dragOverDay = null;
		draggedPlanId = null;
		draggingCropId = null;
	}
</script>

<svelte:head>
	<title>Stardew Crop Planner</title>
	<meta
		name="description"
		content="A clean Stardew Valley crop planner with drag and drop harvest forecasting."
	/>
</svelte:head>

<main
	class="planner-page"
	style={`--season-accent: ${season.accent}; --season-deep: ${season.deep}`}
>
	<header class="top-bar">
		<h1>Crop Planner</h1>
		<div class="summary-strip" aria-label="Season totals">
			<div>
				<span>Items</span>
				<strong>{totals.items}</strong>
			</div>
			<div>
				<span>Revenue</span>
				<strong>{totals.revenue}g</strong>
			</div>
			<div>
				<span>Seeds</span>
				<strong>{totals.seedCost}g</strong>
			</div>
			<div>
				<span>Profit</span>
				<strong>{totals.profit}g</strong>
			</div>
		</div>
	</header>

	<div class="planner-layout">
		<section class="calendar-shell" aria-label="Crop planting calendar">
			<nav class="season-tabs" aria-label="Season selector">
				{#each Object.entries(seasons) as [key, item]}
					<button
						class:active={selectedSeason === key}
						type="button"
						onclick={() => selectSeason(key as Season)}
					>
						{item.name}
					</button>
				{/each}
			</nav>

			<div class="event-filters" aria-label="Calendar event filters">
				{#each Object.entries(eventTypeLabels) as [type, label]}
					<button
						class:active={eventFilters[type as CalendarEvent['type']]}
						type="button"
						onclick={() => toggleEventFilter(type as CalendarEvent['type'])}
					>
						{label}
					</button>
				{/each}
			</div>

			<div
				class="calendar-grid"
				role="none"
				ondragleave={(e) => {
					if (!e.currentTarget.contains(e.relatedTarget as Node)) dragOverDay = null;
				}}
			>
				{#each weekDays as weekDay}
					<div class="weekday">{weekDay}</div>
				{/each}

				{#each days as day}
					{@const planted = plansForDay(day)}
					{@const harvests = harvestsForDay(day)}
					{@const dayEvents = eventsForDay(day)}
					<article
						class="day-card"
						class:drag-over={dragOverDay === day}
						class:harvest-preview={dragPreviewHarvestDays.includes(day)}
						ondrop={(event) => onDrop(event, day)}
						ondragover={(event) => onDragOver(event, day)}
					>
						<span class="day-num">{day}</span>

						<div class="day-stack">
							{#each dayEvents as event}
								<div class={`event-chip event-chip--${event.type}`}>
									{#if event.icon}
										<img alt="" src={event.icon} />
									{:else}
										<span class="event-initial">{event.title.slice(0, 1)}</span>
									{/if}
									<span>{event.title}</span>
								</div>
							{/each}

							{#if dragOverDay === day}
								{@const preview = getDragPreview(day)}
								{#if preview}
									<div class="drop-preview" style={`--crop-color: ${preview.crop.color}`}>
										<img alt={preview.crop.name} src={preview.crop.image} />
										{#if preview.harvestLabels.length}
											<span>{preview.harvestLabels.join(', ')}</span>
										{:else}
											<span>Too late</span>
										{/if}
									</div>
								{/if}
							{/if}

							{#each planted as plan}
								{@const crop = getCrop(plan.cropId)}
								<button
									class="plan-chip"
									draggable="true"
									style={`--crop-color: ${crop.color}`}
									type="button"
									onclick={() => (modalPlanId = plan.id)}
									ondragstart={(event) => onPlanDragStart(event, plan.id)}
									ondragend={onDragEnd}
								>
									<img alt={crop.name} src={crop.image} />
									<span>{crop.name}</span>
									<small>x{plan.quantity}</small>
								</button>
							{/each}

							{#each harvests as harvest}
								<div class="harvest-chip" style={`--crop-color: ${harvest.crop.color}`}>
									<img alt={harvest.crop.name} src={harvest.crop.image} />
									<span>{harvest.crop.name}</span>
									<small>{harvest.items} / {harvest.gold}g</small>
								</div>
							{/each}
						</div>
					</article>
				{/each}
			</div>
		</section>

		<aside class="tool-tab" aria-label="Crop tools">
			<div class="tool-card crop-picker-card">
				<div class="tool-card__header">
					<strong>Crops</strong>
					<div class="header-actions">
						<label class="qty-inline">
							<span>Qty</span>
							<input bind:value={plantQuantity} min="1" type="number" />
						</label>
						<span class="count-badge">{seasonCrops.length}</span>
					</div>
				</div>

				<div class="crop-grid">
					{#each seasonCrops as crop}
						<button
							class:active={selectedCropId === crop.id}
							class="crop-tile"
							draggable="true"
							style={`--crop-color: ${crop.color}`}
							type="button"
							onclick={() => (selectedCropId = crop.id)}
							ondragstart={(event) => onCropDragStart(event, crop.id)}
							ondragend={onDragEnd}
						>
							<span class="crop-tile__icon">
								<img alt={crop.name} src={crop.image} />
							</span>
							<strong class="crop-tile__name">{crop.name}</strong>
							<small class="crop-tile__meta">
								{crop.growDays}d{#if crop.regrowDays} / {crop.regrowDays}d regrow{/if} / {crop.sellPrice}g
							</small>
						</button>
					{/each}
				</div>
			</div>

			<div class="tool-card detail-box" style={`--crop-color: ${selectedCrop.color}`}>
				<div class="detail-hero">
					<span class="detail-hero__icon">
						<img alt={selectedCrop.name} src={selectedCrop.image} />
					</span>
					<div class="detail-hero__text">
						<strong>{selectedCrop.name}</strong>
						<small>
							{selectedCrop.seasons.map((s) => seasons[s].name).join(' & ')}
							{#if selectedCrop.regrowDays} / regrows {selectedCrop.regrowDays}d{/if}
						</small>
					</div>
				</div>
				<div class="stat-pills">
					<div class="stat-pill">
						<span>Grow</span>
						<strong>{selectedCrop.growDays}d</strong>
					</div>
					<div class="stat-pill">
						<span>Yield</span>
						<strong>x{selectedCrop.yieldPerHarvest}</strong>
					</div>
					<div class="stat-pill">
						<span>Seed</span>
						<strong>{selectedCrop.seedPrice}g</strong>
					</div>
					<div class="stat-pill">
						<span>Sell</span>
						<strong>{selectedCrop.sellPrice}g</strong>
					</div>
				</div>
			</div>

			<div class="tool-card plan-list-card">
				<div class="tool-card__header">
					<strong>Plans</strong>
					<span class="count-badge">{visiblePlans.length}</span>
				</div>

				<div class="plan-list">
					{#if visiblePlans.length}
						{#each visiblePlans as plan}
							{@const crop = getCrop(plan.cropId)}
							{@const harvests = getHarvests(plan)}
							{@const summary = getPlanSummary(plan)}
							<div class="plan-row" style={`--crop-color: ${crop.color}`}>
								<img alt={crop.name} src={crop.image} class="plan-row__img" />
								<div class="plan-row__body">
									<strong>{crop.name}</strong>
									<small>
										Day {plan.plantDay}
										{#if harvests.length}
											/ Harvest {summary.harvestDays.join(', ')}
										{:else}
											/ Too late
										{/if}
									</small>
									<div class="plan-metrics">
										<span>{summary.items} items</span>
										<span>{summary.revenue}g rev</span>
										<span class:negative={summary.profit < 0}>{summary.profit}g</span>
									</div>
								</div>
								<div class="plan-row__actions">
									<input
										aria-label={`Quantity for ${crop.name}`}
										min="1"
										type="number"
										value={plan.quantity}
										oninput={(event) =>
											updatePlanQuantity(plan.id, Number(event.currentTarget.value))}
									/>
									<button
										class="remove-btn"
										aria-label={`Remove ${crop.name}`}
										type="button"
										onclick={() => removePlan(plan.id)}
									>
										x
									</button>
								</div>
							</div>
						{/each}
					{:else}
						<p class="empty-state">Drop a crop onto the calendar to start planning.</p>
					{/if}
				</div>
			</div>
		</aside>
	</div>

	{#if modalPlan && modalCrop}
		<div
			class="modal-backdrop"
			role="button"
			tabindex="-1"
			onclick={() => (modalPlanId = null)}
			onkeydown={(e) => e.key === 'Escape' && (modalPlanId = null)}
		>
			<div
				class="modal"
				role="dialog"
				tabindex="0"
				aria-modal="true"
				aria-label={`Edit ${modalCrop.name}`}
				onclick={(e) => e.stopPropagation()}
				onkeydown={(e) => e.key === 'Escape' && (modalPlanId = null)}
			>
				<div class="modal-header" style={`--crop-color: ${modalCrop.color}`}>
					<img alt={modalCrop.name} src={modalCrop.image} />
					<div>
						<strong>{modalCrop.name}</strong>
						<small>Planted {seasons[modalPlan.season].name} {modalPlan.plantDay}</small>
					</div>
					<button class="modal-close" type="button" onclick={() => (modalPlanId = null)}>x</button>
				</div>

				<div class="modal-body">
					<div class="modal-harvests">
						{#if modalHarvests.length}
							{#each modalHarvests as harvest (harvest.absoluteDay)}
								<div class="harvest-badge">
									<span>{seasons[harvest.season].name} {harvest.day}</span>
									<strong>{harvest.items} items / {harvest.gold}g</strong>
								</div>
							{/each}
						{:else}
							<p class="too-late">Won't harvest this season</p>
						{/if}
					</div>

					<label class="modal-qty">
						<span>Quantity</span>
						<input
							type="number"
							min="1"
							value={modalPlan.quantity}
							oninput={(e) => updatePlanQuantity(modalPlan!.id, Number(e.currentTarget.value))}
						/>
					</label>

					<button
						class="modal-remove"
						type="button"
						onclick={() => {
							removePlan(modalPlan!.id);
							modalPlanId = null;
						}}
					>
						Remove crop
					</button>
				</div>
			</div>
		</div>
	{/if}
</main>

<style>
	:global(body) {
		margin: 0;
		color: #2f271f;
		background:
			linear-gradient(180deg, rgba(242, 248, 230, 0.9), rgba(255, 250, 236, 0.96) 320px), #fbf4df;
		font-family:
			Inter,
			ui-sans-serif,
			system-ui,
			-apple-system,
			BlinkMacSystemFont,
			'Segoe UI',
			sans-serif;
	}

	:global(*) {
		box-sizing: border-box;
	}

	button,
	input {
		font: inherit;
	}

	img {
		image-rendering: pixelated;
	}

	h1 {
		margin: 0;
	}

	.planner-page {
		min-height: 100vh;
		padding: 20px;
	}

	/* ── Top bar ── */

	.top-bar {
		display: flex;
		gap: 16px;
		align-items: center;
		justify-content: space-between;
		max-width: 1440px;
		margin: 0 auto 16px;
	}

	h1 {
		color: #2f271f;
		font-size: 1.5rem;
		font-weight: 850;
		white-space: nowrap;
	}

	.event-filters {
		display: flex;
		flex-wrap: wrap;
		gap: 5px;
		padding: 8px 10px;
		border-bottom: 1px solid #e0d4b7;
	}

	.event-filters button {
		padding: 4px 10px;
		border: 1px solid #e0d4b7;
		border-radius: 999px;
		background: #fffdf8;
		color: #8a7a62;
		cursor: pointer;
		font-size: 0.7rem;
		font-weight: 750;
		transition:
			background 0.1s,
			border-color 0.1s,
			color 0.1s;
	}

	.event-filters button.active {
		border-color: color-mix(in srgb, var(--season-accent) 56%, #d0c3a8);
		background: color-mix(in srgb, var(--season-accent) 16%, #fffdf8);
		color: #2f271f;
	}

	.summary-strip {
		display: flex;
		gap: 8px;
		border: 1px solid #e0d4b7;
		border-radius: 8px;
		padding: 8px;
		background: rgba(255, 252, 244, 0.92);
		box-shadow: 0 4px 16px rgba(74, 57, 35, 0.08);
	}

	.summary-strip div {
		padding: 8px 14px;
		border: 1px solid #e4d8bd;
		border-radius: 6px;
		background: #fffaf0;
		text-align: center;
	}

	.summary-strip span,
	.summary-strip strong {
		display: block;
	}

	.summary-strip span {
		color: #8a7a62;
		font-size: 0.65rem;
		font-weight: 800;
		text-transform: uppercase;
	}

	.summary-strip strong {
		margin-top: 3px;
		color: var(--season-deep);
		font-size: 1.1rem;
		font-weight: 800;
	}

	/* ── Layout ── */

	.planner-layout {
		display: grid;
		grid-template-columns: minmax(0, 1fr) 320px;
		gap: 16px;
		max-width: 1440px;
		margin: 0 auto;
	}

	.calendar-shell,
	.tool-tab {
		border: 1px solid #e0d4b7;
		border-radius: 8px;
		background: rgba(255, 252, 244, 0.92);
		box-shadow: 0 4px 20px rgba(74, 57, 35, 0.08);
	}

	/* ── Season tabs ── */

	.season-tabs {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 8px;
		padding: 10px;
		border-bottom: 1px solid #e0d4b7;
	}

	.season-tabs button {
		min-height: 38px;
		border: 1px solid #e0d4b7;
		border-radius: 6px;
		background: #fffaf0;
		color: #655846;
		cursor: pointer;
		font-weight: 700;
		font-size: 0.85rem;
		transition:
			background 0.1s,
			border-color 0.1s;
	}

	.season-tabs button:hover,
	.season-tabs button.active {
		border-color: color-mix(in srgb, var(--season-accent) 64%, #d0c3a8);
		background: color-mix(in srgb, var(--season-accent) 18%, #fffaf0);
		color: #2f271f;
	}

	/* ── Calendar grid ── */

	.calendar-grid {
		display: grid;
		grid-template-columns: repeat(7, minmax(0, 1fr));
		gap: 5px;
		padding: 10px;
		background: #f5ead2;
	}

	.weekday {
		padding: 6px;
		border: 1px solid #e0d4b7;
		border-radius: 5px;
		background: #e7f1dd;
		color: #526345;
		font-size: 0.68rem;
		font-weight: 800;
		text-align: center;
		text-transform: uppercase;
	}

	.day-card {
		display: flex;
		min-height: 110px;
		flex-direction: column;
		gap: 5px;
		padding: 6px;
		border: 1px solid #e0d4b7;
		border-radius: 6px;
		background: #fffdf8;
	}

	.day-card:hover,
	.day-card.drag-over {
		background: color-mix(in srgb, var(--season-accent) 14%, #fff8d8);
	}

	.day-card.drag-over {
		border-color: color-mix(in srgb, var(--season-accent) 70%, #d0c3a8);
		border-style: dashed;
	}

	.day-card.harvest-preview {
		background: #eef7e6;
		border-color: #8fbd72;
	}

	.day-card.harvest-preview .day-num {
		background: #c4e8ac;
		color: #2d5a1b;
	}

	.drop-preview {
		display: flex;
		gap: 5px;
		align-items: center;
		padding: 4px 6px;
		border: 1.5px dashed color-mix(in srgb, var(--crop-color) 60%, #b0a080);
		border-radius: 4px;
		background: color-mix(in srgb, var(--crop-color) 10%, #fffdf8);
		color: #534431;
		font-size: 0.68rem;
		font-weight: 700;
		pointer-events: none;
	}

	.drop-preview img {
		width: 14px;
		height: 14px;
		object-fit: contain;
		opacity: 0.7;
	}

	.drop-preview span {
		opacity: 0.8;
	}

	.day-num {
		display: inline-grid;
		width: 22px;
		aspect-ratio: 1;
		place-items: center;
		border-radius: 4px;
		background: #f1e4c9;
		color: #534431;
		font-size: 0.72rem;
		font-weight: 850;
	}

	.day-stack {
		display: grid;
		gap: 3px;
		align-content: start;
	}

	.plan-chip,
	.harvest-chip,
	.event-chip {
		display: grid;
		grid-template-columns: 16px minmax(0, 1fr) auto;
		gap: 2px 4px;
		align-items: center;
		width: 100%;
		min-width: 0;
		min-height: 24px;
		padding: 3px 4px;
		border: 1px solid color-mix(in srgb, var(--crop-color) 46%, #d9cdb4);
		border-radius: 4px;
		background: color-mix(in srgb, var(--crop-color) 16%, #fffdf8);
		color: #2f271f;
		text-align: left;
	}

	.plan-chip {
		cursor: grab;
	}

	.plan-chip img,
	.harvest-chip img,
	.event-chip img {
		width: 16px;
		height: 16px;
		object-fit: contain;
	}

	.plan-chip span,
	.harvest-chip span,
	.event-chip span,
	.plan-chip small,
	.harvest-chip small {
		display: block;
		overflow-wrap: anywhere;
	}

	.plan-chip span,
	.harvest-chip span,
	.event-chip span {
		overflow: hidden;
		font-size: 0.68rem;
		font-weight: 750;
		line-height: 1.1;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.plan-chip small,
	.harvest-chip small {
		font-size: 0.6rem;
		color: #7a6a52;
		white-space: nowrap;
	}

	.harvest-chip {
		border-color: #bed3a7;
		background: #eef7e6;
	}

	.harvest-chip small {
		color: #597143;
	}

	.event-chip {
		grid-template-columns: 16px minmax(0, 1fr);
		border-color: #ded3bd;
		background: #f8f1e4;
		color: #655846;
	}

	.event-chip--festival {
		border-color: #e5c26d;
		background: #fff6d8;
	}

	.event-chip--birthday {
		border-color: #dfb7d3;
		background: #fff0fa;
	}

	.event-chip--shop {
		border-color: #d9cdb4;
		background: #f4eee3;
	}

	.event-chip--season {
		border-color: #b8d5a0;
		background: #eff8e8;
	}

	.event-chip--special,
	.event-chip--bookseller {
		border-color: #c8bddf;
		background: #f2effa;
	}

	.event-initial {
		display: inline-grid;
		width: 16px;
		height: 16px;
		place-items: center;
		border-radius: 4px;
		background: rgba(101, 88, 70, 0.12);
		font-size: 0.62rem;
		font-weight: 850;
	}

	/* ── Tool tab ── */

	.tool-tab {
		position: sticky;
		top: 16px;
		align-self: start;
		max-height: calc(100vh - 32px);
		overflow: hidden;
		padding: 10px;
		display: flex;
		flex-direction: column;
		gap: 10px;
	}

	.tool-card {
		display: flex;
		min-height: 0;
		flex-direction: column;
		gap: 10px;
		padding: 10px;
		border: 1px solid #e0d4b7;
		border-radius: 8px;
		background: #fffdf8;
	}

	.tool-card__header {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 8px;
		padding-left: 8px;
		border-left: 3px solid var(--season-accent);
		color: #2f271f;
		font-size: 0.84rem;
	}

	.count-badge {
		display: inline-grid;
		min-width: 22px;
		height: 20px;
		place-items: center;
		border-radius: 999px;
		background: #f1e4c9;
		color: #655846;
		font-size: 0.68rem;
		font-weight: 800;
	}

	.header-actions {
		display: flex;
		align-items: center;
		gap: 6px;
	}

	.qty-inline {
		display: flex;
		align-items: center;
		gap: 4px;
	}

	.qty-inline span {
		color: #8a7a62;
		font-size: 0.65rem;
		font-weight: 800;
		text-transform: uppercase;
		white-space: nowrap;
	}

	.qty-inline input {
		width: 48px;
		padding: 3px 5px;
		border: 1px solid #d9cdb4;
		border-radius: 5px;
		background: #fffdf8;
		color: #2f271f;
		font-size: 0.78rem;
		font-weight: 700;
		text-align: center;
	}

	.crop-picker-card {
		flex: 0 0 auto;
	}

	.plan-list-card {
		flex: 1 1 44%;
	}

	input[type='number'] {
		width: 100%;
		min-width: 0;
		padding: 6px 8px;
		border: 1px solid #d9cdb4;
		border-radius: 6px;
		background: #fffdf8;
		color: #2f271f;
		font-weight: 700;
	}

	/* ── Crop grid ── */

	.crop-grid {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr;
		gap: 4px;
		height: 204px;
		overflow-y: auto;
		padding-right: 2px;
	}

	.crop-tile {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 3px;
		padding: 6px 4px;
		border: 1px solid #e0d4b7;
		border-radius: 6px;
		background: #fffdf8;
		color: #2f271f;
		cursor: grab;
		text-align: center;
		transition:
			background 0.1s,
			border-color 0.1s;
	}

	.crop-tile.active,
	.crop-tile:hover {
		border-color: color-mix(in srgb, var(--crop-color) 55%, #d9cdb4);
		background: color-mix(in srgb, var(--crop-color) 18%, #fffdf8);
	}

	.crop-tile__icon {
		display: grid;
		width: 28px;
		aspect-ratio: 1;
		place-items: center;
		border-radius: 5px;
		background: color-mix(in srgb, var(--crop-color) 22%, #fffdf8);
	}

	.crop-tile__icon img {
		width: 22px;
		height: 22px;
		object-fit: contain;
	}

	.crop-tile__name {
		display: block;
		font-size: 0.68rem;
		line-height: 1.2;
	}

	.crop-tile__meta {
		display: block;
		color: #8a7a62;
		font-size: 0.62rem;
		font-weight: 700;
	}

	/* ── Detail box ── */

	.detail-box {
		background: #fff7e6;
		border-color: color-mix(in srgb, var(--crop-color, #e0d4b7) 30%, #e0d4b7);
		flex: 0 0 auto;
	}

	.detail-hero {
		display: flex;
		gap: 10px;
		align-items: center;
	}

	.detail-hero__icon {
		display: grid;
		flex-shrink: 0;
		width: 40px;
		aspect-ratio: 1;
		place-items: center;
		border-radius: 8px;
		background: color-mix(in srgb, var(--crop-color) 18%, #fffdf8);
	}

	.detail-hero__icon img {
		width: 30px;
		height: 30px;
		object-fit: contain;
	}

	.detail-hero__text strong {
		display: block;
		font-size: 0.9rem;
	}

	.detail-hero__text small {
		display: block;
		margin-top: 2px;
		color: #8a7a62;
		font-size: 0.65rem;
		font-weight: 700;
	}

	.stat-pills {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 4px;
	}

	.stat-pill {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 5px 8px;
		border: 1px solid #e0d4b7;
		border-radius: 5px;
		background: #fffdf8;
	}

	.stat-pill span {
		color: #8a7a62;
		font-size: 0.62rem;
		font-weight: 800;
		text-transform: uppercase;
	}

	.stat-pill strong {
		color: #2f271f;
		font-size: 0.8rem;
		font-weight: 800;
	}

	/* ── Plan list ── */

	.plan-list {
		display: grid;
		flex: 1 1 auto;
		gap: 6px;
		min-height: 216px;
		overflow-y: auto;
		padding-right: 4px;
	}

	.crop-grid::-webkit-scrollbar,
	.plan-list::-webkit-scrollbar {
		width: 6px;
	}

	.crop-grid::-webkit-scrollbar-track,
	.plan-list::-webkit-scrollbar-track {
		border-radius: 999px;
		background: #f3ead8;
	}

	.crop-grid::-webkit-scrollbar-thumb,
	.plan-list::-webkit-scrollbar-thumb {
		border: 2px solid #f3ead8;
		border-radius: 999px;
		background: #c8b895;
	}

	.empty-state {
		margin: 0;
		padding: 12px;
		border: 1px dashed #d9cdb4;
		border-radius: 6px;
		background: #fffaf0;
		color: #8a7a62;
		font-size: 0.78rem;
		line-height: 1.4;
	}

	.plan-row {
		display: grid;
		grid-template-columns: 22px 1fr auto;
		gap: 6px;
		align-items: start;
		padding: 8px;
		border: 1px solid color-mix(in srgb, var(--crop-color) 44%, #d9cdb4);
		border-radius: 6px;
		background: color-mix(in srgb, var(--crop-color) 10%, #fffdf8);
	}

	.plan-row__img {
		width: 22px;
		height: 22px;
		margin-top: 2px;
		object-fit: contain;
	}

	.plan-row__body {
		min-width: 0;
	}

	.plan-row__body strong {
		display: block;
		font-size: 0.8rem;
	}

	.plan-row__body small {
		display: block;
		margin-top: 2px;
		color: #8a7a62;
		font-size: 0.64rem;
		font-weight: 700;
	}

	.plan-row__actions {
		display: flex;
		flex-direction: column;
		gap: 3px;
		align-items: center;
	}

	.plan-row__actions input[type='number'] {
		width: 44px;
		padding: 3px 4px;
		font-size: 0.75rem;
		text-align: center;
	}

	.remove-btn {
		display: grid;
		width: 26px;
		aspect-ratio: 1;
		place-items: center;
		border: 1px solid #e5b6aa;
		border-radius: 5px;
		background: #fff2ef;
		color: #a33c2f;
		cursor: pointer;
		font-size: 1rem;
		font-weight: 800;
	}

	.plan-metrics {
		display: flex;
		flex-wrap: wrap;
		gap: 3px;
		margin-top: 4px;
	}

	.plan-metrics span {
		padding: 2px 5px;
		border: 1px solid #e0d4b7;
		border-radius: 999px;
		background: rgba(255, 253, 248, 0.72);
		color: #655846;
		font-size: 0.58rem;
		font-weight: 750;
		white-space: nowrap;
	}

	.plan-metrics .negative {
		border-color: #e5b6aa;
		background: #fff2ef;
		color: #a33c2f;
	}

	/* ── Modal ── */

	.modal-backdrop {
		position: fixed;
		inset: 0;
		display: grid;
		place-items: center;
		background: rgba(30, 22, 12, 0.45);
		z-index: 100;
		padding: 16px;
	}

	.modal {
		width: 100%;
		max-width: 340px;
		border: 1px solid #e0d4b7;
		border-radius: 10px;
		background: #fffdf8;
		box-shadow: 0 16px 48px rgba(30, 22, 12, 0.22);
		overflow: hidden;
		outline: none;
	}

	.modal-header {
		display: flex;
		gap: 10px;
		align-items: center;
		padding: 14px 16px;
		border-bottom: 1px solid #e0d4b7;
		background: color-mix(in srgb, var(--crop-color) 12%, #fff7e6);
	}

	.modal-header img {
		width: 36px;
		height: 36px;
		object-fit: contain;
	}

	.modal-header div {
		flex: 1;
	}

	.modal-header strong {
		display: block;
		font-size: 1rem;
		color: #2f271f;
	}

	.modal-header small {
		color: #8a7a62;
		font-size: 0.7rem;
		font-weight: 700;
	}

	.modal-close {
		width: 28px;
		height: 28px;
		display: grid;
		place-items: center;
		border: 1px solid #e0d4b7;
		border-radius: 6px;
		background: #fffdf8;
		color: #655846;
		cursor: pointer;
		font-size: 1.1rem;
	}

	.modal-body {
		padding: 14px 16px;
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	.modal-harvests {
		display: flex;
		flex-direction: column;
		gap: 6px;
	}

	.harvest-badge {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 7px 10px;
		border: 1px solid #bed3a7;
		border-radius: 6px;
		background: #eef7e6;
	}

	.harvest-badge span {
		color: #526345;
		font-size: 0.75rem;
		font-weight: 800;
		text-transform: uppercase;
	}

	.harvest-badge strong {
		color: #2d5a1b;
		font-size: 0.82rem;
	}

	.too-late {
		color: #a05a40;
		font-size: 0.82rem;
		font-weight: 600;
		margin: 0;
	}

	.modal-qty {
		display: flex;
		gap: 10px;
		align-items: center;
	}

	.modal-qty span {
		color: #8a7a62;
		font-size: 0.72rem;
		font-weight: 800;
		text-transform: uppercase;
		white-space: nowrap;
	}

	.modal-remove {
		width: 100%;
		padding: 9px;
		border: 1px solid #e5b6aa;
		border-radius: 6px;
		background: #fff2ef;
		color: #a33c2f;
		cursor: pointer;
		font-weight: 700;
		font-size: 0.85rem;
	}

	.modal-remove:hover {
		background: #fde0da;
	}

	/* ── Responsive ── */

	@media (max-width: 1100px) {
		.planner-layout {
			grid-template-columns: 1fr;
		}

		.tool-tab {
			position: relative;
			top: 0;
			max-height: none;
		}
	}

	@media (max-width: 720px) {
		.planner-page {
			padding: 10px;
		}

		.top-bar {
			flex-direction: column;
			align-items: flex-start;
			gap: 10px;
		}

		.summary-strip {
			width: 100%;
			display: grid;
			grid-template-columns: repeat(4, 1fr);
		}

.season-tabs {
			grid-template-columns: repeat(2, 1fr);
		}

		.calendar-grid {
			overflow-x: auto;
			grid-template-columns: repeat(7, minmax(96px, 1fr));
			padding: 8px;
		}
	}
</style>
