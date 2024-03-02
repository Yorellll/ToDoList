<script>
  import { Route, Router } from "svelte-routing";
  import Dashboard from "./lib/Dashboard.svelte";
  import FormListe from "./lib/FormListe.svelte";
  import CreateTodo from "./routes/CreateTodo.svelte";
</script>

<Router>
  <Route path="/" component={Dashboard} />
  <Route path="/:list">
    <CreateTodo />
  </Route>
  <Route path="/create" component={FormListe} />
</Router>
