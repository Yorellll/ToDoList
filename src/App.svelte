<script>
  import { Link, Route, Router } from "svelte-routing";
  import Dashboard from "./lib/Dashboard.svelte";
  import CreateTodo from "./routes/CreateTodo.svelte";
</script>

<Router>
  <Route path="/" component={Dashboard} />
  <Route path="/create" component={CreateTodo} />
</Router>
