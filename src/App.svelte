<script>
  import { Route, Router } from "svelte-routing";
  import Dashboard from "./lib/Dashboard.svelte";
  import FormListe from "./lib/FormListe.svelte";
  import Hello from "./lib/Hello.svelte";
</script>

<Router>
  <Route path="/" component={Dashboard} />
  <Route path="/:" component={Hello} />
  <Route path="/create" component={FormListe} />
</Router>
