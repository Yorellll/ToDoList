<script>
  import { Route, Router } from "svelte-routing";
  import Dashboard from "./lib/Dashboard.svelte";
  import FormListe from "./lib/FormListe.svelte";
</script>

<Router>
  <Route path="/" component={Dashboard} />
  <Route path="/create" component={FormListe} />
</Router>
