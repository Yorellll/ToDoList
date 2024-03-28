<script>
    import {Route, Router} from "svelte-routing";
    import Header from "./layouts/Header.svelte";
    import Dashboard from "./lib/Dashboard.svelte";
    import FormList from "./lib/FormList.svelte";
    import CreateTodo from "./routes/CreateTodo.svelte";
</script>

<Header/>
<Router>
    <Route path="/" component={Dashboard}/>
    <Route path="/:todo">
        <CreateTodo/>
    </Route>
    <Route path="/:todo/:subTodo">
        <CreateTodo/>
    </Route>
    <Route path="/create" component={FormList}/>
</Router>
