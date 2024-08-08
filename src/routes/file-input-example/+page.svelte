<script lang="ts">
	import * as Accordion from '$lib/components/ui/accordion/index.js';
	import { Input } from '$lib/components/ui/input';
	import SuperDebug, { fileProxy, filesProxy, superForm } from 'sveltekit-superforms';
	import { FormControl, FormButton, FormField, FormLabel } from '$lib/components/ui/form';
	import { FormFieldErrors } from '$lib/components/ui/form/index.js';

	export let data;

	const form = superForm(data.form);
	const { form: formData } = form;

	import * as Select from '$lib/components/ui/select/index.js';
	import { onMount } from 'svelte';

	const fruits = [
		{ value: 'apple', label: 'Apple' },
		{ value: 'banana', label: 'Banana' },
		{ value: 'blueberry', label: 'Blueberry' },
		{ value: 'grapes', label: 'Grapes' },
		{ value: 'pineapple', label: 'Pineapple' }
	];

	let days: any[] = [];
	let months: any[] = [];
	let years: any[] = [];
	let countries = [
		'United States',
		'Canada',
		'United Kingdom',
		'Australia',
		'India',
		'Germany',
		'France',
		'Japan',
		'China',
		'Brazil'
	];
	let documentTypes = ['Passport', "Driver's License", 'ID Card', 'Birth Certificate', 'Other'];

	// Generate days and months
	onMount(() => {
		days = Array.from({ length: 31 }, (_, i) => i + 1);
		months = Array.from({ length: 12 }, (_, i) => i + 1);
	});

	// Generate years dynamically based on the current year
	const currentYear = new Date().getFullYear();
	years = Array.from({ length: 100 }, (_, i) => currentYear - i);

	// Create a reference for the file input
	let fileInput: HTMLInputElement | null = null;

	// Reactive variable for the selected files and image URL
	let files: FileList | null = null;
	let imageUrl: string | null = null;

	// Function to handle button click
	function triggerFileInput() {
		fileInput?.click();
	}

	// Function to handle file input change
	function handleFileChange(event: Event) {
		const input = event.target as HTMLInputElement;
		files = input.files;
		if (files && files[0]) {
			const file = files[0];
			imageUrl = URL.createObjectURL(file);
		} else {
			imageUrl = null;
		}
	}
</script>

<Accordion.Root class="mx-auto w-full sm:max-w-[70%]">
	<Accordion.Item class="mb-[10px] rounded-[10px] border-0 bg-[#17141f] p-3" value="item-1">
		<Accordion.Trigger class="p-0 text-[18px] font-semibold text-white hover:no-underline">
			<div class="flex items-center gap-4">
				Level 1
				<p class="rounded-[5px] bg-[#86d77f36] px-[8px] py-[5px] text-[13px] text-[#87d77f]">
					Verified
				</p>
			</div></Accordion.Trigger
		>
		<Accordion.Content class="mt-[15px]">
			<form method="POST" enctype="multipart/form-data" class=" space-y-4">
				<div class="flex gap-4">
					<FormField class="w-1/2" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white">
								first name <span class="!text-[red]">*</span></FormLabel
							>
							<Input
								class="text-white placeholder:text-[#fff]"
								placeholder="First Name"
								{...attrs}
							/>
							<FormFieldErrors />
						</FormControl>
					</FormField>
					<FormField class="w-1/2" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white"
								>last name <span class="!text-[red]">*</span></FormLabel
							>
							<Input
								class="text-white placeholder:text-[#fff]"
								placeholder="Last Name"
								{...attrs}
							/>
							<FormFieldErrors />
						</FormControl>
					</FormField>
				</div>

				<div class="flex gap-4">
					<div class="w-full">
						<div class="mb-[8px]">
							<label class="font-medium font-medium uppercase text-white" for=""
								>Date Of Birth <span class="!text-[red]">*</span></label
							>
						</div>
						<Select.Root portal={null}>
							<Select.Trigger class="w-full py-1 text-white">
								<Select.Value class="custom-placeholder" placeholder="Please Select" />
							</Select.Trigger>
							<Select.Content class="max-h-60 overflow-scroll">
								<Select.Group>
									<Select.Label>Select</Select.Label>
									{#each days as day}
										<Select.Item class="" value={day} label={day}>{day}</Select.Item>
									{/each}
								</Select.Group>
							</Select.Content>
							<Select.Input name="date" />
						</Select.Root>
					</div>
					<div class="w-full">
						<div class="mb-[8px]">
							<label class="font-medium uppercase text-white" for=""
								>Month <span class="!text-[red]">*</span></label
							>
						</div>
						<Select.Root portal={null}>
							<Select.Trigger class="w-full text-white">
								<Select.Value placeholder="Month" />
							</Select.Trigger>
							<Select.Content class="max-h-60 overflow-scroll">
								<Select.Group>
									<Select.Label>Month</Select.Label>
									{#each months as month}
										<Select.Item value={month} label={month}>{month}</Select.Item>
									{/each}
								</Select.Group>
							</Select.Content>
							<Select.Input name="month" />
						</Select.Root>
					</div>
					<div class="w-full">
						<div class="mb-[8px]">
							<label class="font-medium uppercase text-white" for=""
								>Year <span class="!text-[red]">*</span></label
							>
						</div>
						<Select.Root portal={null}>
							<Select.Trigger class="w-full text-white">
								<Select.Value placeholder="Select Year" />
							</Select.Trigger>
							<Select.Content class="max-h-60 overflow-scroll">
								<Select.Group>
									<Select.Label>Year</Select.Label>
									{#each years as year}
										<Select.Item value={year} label={year}>{year}</Select.Item>
									{/each}
								</Select.Group>
							</Select.Content>
							<Select.Input name="year" />
						</Select.Root>
					</div>
				</div>
				<div>
					<FormField class="w-full" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white"
								>Address Line 1 <span class="!text-[red]">*</span></FormLabel
							>
							<Input
								class="text-white placeholder:text-[#fff]"
								placeholder="Address Line 1"
								{...attrs}
							/>
							<!-- <Input {...attrs} /> -->
							<FormFieldErrors />
						</FormControl>
					</FormField>
				</div>
				<div>
					<FormField class="w-full" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white">Address Line 2</FormLabel>
							<Input
								class="text-white placeholder:text-[#fff]"
								placeholder="Address Line 2"
								{...attrs}
							/>
							<FormFieldErrors />
						</FormControl>
					</FormField>
				</div>
				<div class="flex gap-4">
					<FormField class="w-1/2" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white"
								>City <span class="!text-[red]">*</span></FormLabel
							>
							<Input class="text-white placeholder:text-[#fff]" placeholder="City" {...attrs} />
							<FormFieldErrors />
						</FormControl>
					</FormField>
					<FormField class="w-1/2" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white"
								>Province <span class="!text-[red]">*</span></FormLabel
							>
							<Input class="text-white placeholder:text-[#fff]" placeholder="Province" {...attrs} />
							<FormFieldErrors />
						</FormControl>
					</FormField>
				</div>

				<div class="flex gap-4">
					<FormField class="w-1/2" {form} name="name">
						<FormControl let:attrs>
							<FormLabel class="uppercase text-white"
								>Postal Code <span class="!text-[red]">*</span></FormLabel
							>
							<Input
								class="text-white placeholder:text-[#fff]"
								placeholder="Postal Code"
								{...attrs}
							/>

							<FormFieldErrors />
						</FormControl>
					</FormField>
					<div class="w-1/2">
						<div class="mb-[8px]">
							<label class="font-medium uppercase text-white" for=""
								>Country <span class="!text-[red]">*</span></label
							>
						</div>
						<Select.Root portal={null}>
							<Select.Trigger class="w-full text-white">
								<Select.Value placeholder="Select a country" />
							</Select.Trigger>
							<Select.Content class="max-h-60 overflow-scroll">
								<Select.Group>
									<Select.Label>Country</Select.Label>
									{#each countries as country}
										<Select.Item value={country} label={country}>{country}</Select.Item>
									{/each}
								</Select.Group>
							</Select.Content>
							<Select.Input name="country" />
						</Select.Root>
					</div>
				</div>

				<FormButton class="w-full bg-[#ff00f8] hover:bg-[#ff00f8]">Submit</FormButton>
			</form>
		</Accordion.Content>
	</Accordion.Item>
	<Accordion.Item class="mb-[10px] rounded-[10px] border-0 bg-[#17141f] p-3" value="item-2">
		<Accordion.Trigger class="p-0 text-[18px] font-semibold text-white hover:no-underline"
			><div class="flex items-center gap-4">
				Level 2
				<p class="rounded-[5px] bg-[#ff000021] px-[8px] py-[5px] text-[13px] text-[#FF0000]">
					Required
				</p>
			</div></Accordion.Trigger
		>
		<Accordion.Content class="p-0">
			<div class="mt-[15px]">
				<h1 class="mb-[15px] text-xl font-medium text-white">Upload identification</h1>
				<p class="mb-[10px] text-white">
					Please upload your identification. This step will unlock more capabilities such as higher
					betting limits and enhanced account security.
				</p>
				<div class="w-full">
					<div class="mb-[8px]">
						<label class="font-medium text-white" for=""
							>Document type <span class="!text-[red]">*</span></label
						>
					</div>
					<Select.Root portal={null}>
						<Select.Trigger class="w-full py-1 text-white">
							<Select.Value class="placeholder:text-red-500" placeholder="Select a fruit" />
						</Select.Trigger>
						<Select.Content class="max-h-60 overflow-scroll">
							<Select.Group>
								<Select.Label>Document</Select.Label>
								{#each documentTypes as document}
									<Select.Item class="" value={document} label={document}>{document}</Select.Item>
								{/each}
							</Select.Group>
						</Select.Content>
						<Select.Input name="document" />
					</Select.Root>
				</div>
				<div class="mt-2">
					<p class="mb-[10px] rounded-[10px] border-0 bg-[#6c5c6c] p-3 text-white">
						Following file types are accepted: ,png, .jpg, .pdf
					</p>
				</div>

				<div class="mb-[10px] flex gap-[10px]">
					<div class="w-[50%]">
						<h1 class="pb-[5px] font-medium text-white">
							Front side <span class="!text-[red]">*</span>
						</h1>
						<div
							class="bg-image flex h-[150px] w-full flex-col items-center justify-center bg-[#0b0814]"
						>
							<input type="file" bind:this={fileInput} hidden on:change={handleFileChange} />
							<img
								class="mb-[10px] h-[60px] w-[60px]"
								src="https://i.ibb.co/9bSGL6T/7141292-1-removebg-preview.png"
								alt="Italian Trulli"
							/>
							<button
								class="rounded-[4px] bg-[#ff00f8] px-[15px] py-[8px] text-[12px] text-white"
								on:click={triggerFileInput}>Upload Front Side</button
							>
						</div>
					</div>
					<div class="w-[50%]">
						<h1 class="pb-[5px] font-medium text-white">
							Back side <span class="!text-[red]">*</span>
						</h1>
						<div
							class="bg-image flex h-[150px] w-full flex-col items-center justify-center bg-[#0b0814]"
						>
							<img
								class="mb-[10px] h-[60px] w-[60px]"
								src="https://i.ibb.co/9bSGL6T/7141292-1-removebg-preview.png"
								alt="Italian Trulli"
							/>
							<input type="file" bind:this={fileInput} hidden on:change={handleFileChange} />
							<button
								class="rounded-[4px] bg-[#ff00f8] px-[15px] py-[8px] text-[12px] text-white"
								on:click={triggerFileInput}>Upload Back Side</button
							>
						</div>
					</div>

					<!-- <FormField {form} name="file">
						<FormControl let:attrs>
							<FormLabel>Front side</FormLabel>
							<Input {...attrs} type="file" bind:files={$file} />
							<FormFieldErrors />
						</FormControl>
					</FormField>

					<FormField {form} name="files">
						<FormControl let:attrs>
							<FormLabel>Back side</FormLabel>
							<Input {...attrs} type="file" bind:files={$files} multiple />
							<FormFieldErrors />
						</FormControl>
					</FormField> -->
				</div>
				<FormButton class="w-full bg-[#ff00f8] hover:bg-[#ff00f8]">Submit</FormButton>
			</div>
		</Accordion.Content>
	</Accordion.Item>
	<Accordion.Item class="mb-[10px] rounded-[10px] border-0 bg-[#17141f] p-3" value="item-3">
		<Accordion.Trigger class="p-0 text-[18px] font-semibold text-white hover:no-underline"
			><div class="flex items-center gap-4">
				Level 3
				<p class="rounded-[5px] bg-[#ff000021] px-[8px] py-[5px] text-[13px] text-[#FF0000]">
					Required
				</p>
			</div></Accordion.Trigger
		>
		<Accordion.Content class="p-0">
			<div class="mt-[15px]">
				<h1 class="mb-[15px] text-xl font-medium text-white">Verification</h1>
				<p class="mb-[10px] text-white">
					Please upload your proof of address. All documents must be laying on a flat surface with
					all 4 corners insides the frame. All information should be clear and identidiable
				</p>

				<p
					class="mb-[5px] rounded-[5px] border-[1px] border-dashed border-[yellow] bg-[#00000045] p-3 text-white"
				>
					Please complete level two verification first.
				</p>
				<div class="mt-2">
					<div class="mb-[8px]">
						<label class="font-medium uppercase text-white" for=""
							>Proff of Address <span class="!text-[red]">*</span></label
						>
					</div>
					<p class="mb-[10px] rounded-[10px] border-0 bg-[#6c5c6c] p-3 text-white">
						Following file types are accepted: ,png, .jpg, .pdf
					</p>
				</div>

				<div class="mb-[10px] flex gap-[10px]">
					<div class="w-full">
						<h1 class="pb-[5px] font-light font-medium text-white">Front side</h1>
						<div
							class="bg-image flex h-[150px] w-full flex-col items-center justify-center bg-[#0b0814]"
						>
							<img
								class="mb-[10px] h-[60px] w-[60px]"
								src="https://i.ibb.co/9bSGL6T/7141292-1-removebg-preview.png"
								alt="Italian Trulli"
							/>
							<input type="file" bind:this={fileInput} hidden on:change={handleFileChange} />
							<button
								class="rounded-[4px] bg-[#ff00f8] px-[15px] py-[8px] text-[12px] text-white"
								on:click={triggerFileInput}>Upload Front Side</button
							>
						</div>
					</div>
					<!-- <FormField {form} name="file">
						<FormControl let:attrs>
							<FormLabel>Front side</FormLabel>
							<Input {...attrs} type="file" bind:files={$file} />
							<FormFieldErrors />
						</FormControl>
					</FormField>

					<FormField {form} name="files">
						<FormControl let:attrs>
							<FormLabel>Back side</FormLabel>
							<Input {...attrs} type="file" bind:files={$files} multiple />
							<FormFieldErrors />
						</FormControl>
					</FormField> -->
				</div>
				<FormButton class="w-full bg-[#ff00f8] hover:bg-[#ff00f8]">Submit</FormButton>
			</div>
		</Accordion.Content>
	</Accordion.Item>
	<Accordion.Item class="mb-[10px] rounded-[10px] border-0 bg-[#17141f] p-3" value="item-4">
		<Accordion.Trigger class="p-0 text-[18px] font-semibold text-white hover:no-underline"
			><div class="flex items-center gap-4">
				Level 4
				<p class="rounded-[5px] bg-[#ff000021] px-[8px] py-[5px] text-[13px] text-[#FF0000]">
					Required
				</p>
			</div></Accordion.Trigger
		>
		<Accordion.Content class="p-0">
			<div class="mt-[15px]">
				<h1 class="mb-[15px] text-xl font-medium text-white">Verification</h1>
				<p class="mb-[10px] text-white">
					Please upload supporting documentation for your source of funds. Document laying on a flat
					surface must show all 4 corners and all information should be clear and identifiable.
				</p>

				<p
					class="mb-[10px] rounded-[5px] border-[1px] border-dashed border-[yellow] bg-[#00000045] p-3 text-white"
				>
					Please complete level three verification first.
				</p>
				<div class="mt-2">
					<div class="mb-[8px]">
						<label class="font-medium uppercase text-white" for=""
							>Source of Funds <span class="!text-[red]">*</span></label
						>
					</div>
					<p class="mb-[10px] rounded-[10px] border-0 bg-[#6c5c6c] p-3 text-white">
						Following file types are accepted: ,png, .jpg, .pdf
					</p>
				</div>

				<div class="mb-[10px] flex gap-[10px]">
					<div class="w-full">
						<h1 class="pb-[5px] font-light font-medium text-white">Front side</h1>
						<div class="bg-image flex h-[150px] w-full flex-col items-center justify-center bg-[#0b0814]">
						  {#if imageUrl}
							<img class="mb-[10px] h-[60px] w-[60px]" src={imageUrl} alt="Selected image" />
						  {:else}
							<img class="mb-[10px] h-[60px] w-[60px]" src="https://i.ibb.co/9bSGL6T/7141292-1-removebg-preview.png" alt="Placeholder" />
						  {/if}
						  <input type="file" bind:this={fileInput} hidden on:change={handleFileChange} />
						  <button class="rounded-[4px] bg-[#ff00f8] px-[15px] py-[8px] text-[12px] text-white" on:click={triggerFileInput}>
							Upload Front Side
						  </button>
						</div>
					  </div>
					<!-- <FormField {form} name="file">
						<FormControl let:attrs>
							<FormLabel>Front side</FormLabel>
							<Input {...attrs} type="file" bind:files={$file} />
							<FormFieldErrors />
						</FormControl>
					</FormField>

					<FormField {form} name="files">
						<FormControl let:attrs>
							<FormLabel>Back side</FormLabel>
							<Input {...attrs} type="file" bind:files={$files} multiple />
							<FormFieldErrors />
						</FormControl>
					</FormField> -->
				</div>
				<FormButton class="w-full bg-[#ff00f8] hover:bg-[#ff00f8]">Submit</FormButton>
			</div>
		</Accordion.Content>
	</Accordion.Item>
</Accordion.Root>

<style>
	.bg-image {
		background-image: url("data:image/svg+xml,%3csvg width='100%25' height='100%25' xmlns='http://www.w3.org/2000/svg'%3e%3crect width='100%25' height='100%25' fill='none' stroke='white' stroke-width='3' stroke-dasharray='6%2c 14' stroke-dashoffset='22' stroke-linecap='square'/%3e%3c/svg%3e");
	}

	.bg-image-second {
		background-image: url("data:image/svg+xml,%3csvg width='100%25' height='100%25' xmlns='http://www.w3.org/2000/svg'%3e%3crect width='100%25' height='100%25' fill='none' stroke='yellow' stroke-width='3' stroke-dasharray='6%2c 14' stroke-dashoffset='22' stroke-linecap='square'/%3e%3c/svg%3e");
	}

	.custom-placeholder::placeholder {
		color: #ff5722;
		opacity: 1;
	}
</style>
