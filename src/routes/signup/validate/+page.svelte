<!-- routes/signup/+page.svelte -->
<script lang="ts">
  import * as Form from "$lib/components/ui/form";
  import { Input } from "$lib/components/ui/input";
  import { validateEmailSchema, type ValidateEmailSchema } from "../schema";
  import { page } from '$app/stores';
  import {
    type SuperValidated,
    type Infer,
    superForm,
  } from "sveltekit-superforms";
  import { zodClient } from "sveltekit-superforms/adapters";

  export let data: SuperValidated<Infer<ValidateEmailSchema>>;

  const form = superForm(data, {
    validators: zodClient(validateEmailSchema),
    dataType: "json",
  });

  const { form: formData, enhance, message } = form;

</script>
<h1>Sign up</h1>
<form method="POST" use:enhance>
  <Form.Field {form} name="code">
    <Form.Control let:attrs>
      <Form.Label>Validation Code</Form.Label>
      <Input {...attrs} bind:value={$formData.code} />
    </Form.Control>
    <Form.Description>Please enter your validation code to finalize your signup</Form.Description>
    <Form.FieldErrors />
  </Form.Field>
  <Form.Button>Submit</Form.Button>

  {#if $message}
  <div 
    class:success={$page.status == 200} 
    class:error={$page.status >= 400}
  >
    {$message}
  </div>
{/if}

</form>

