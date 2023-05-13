# Go dependencies analyze

There could be an issue with rich dependency tree in some Go project.
So it's worth having some tools to analyze them.

Two different tools are described here, they could be installed with `make tools` command.

## Analyze of regular dependency resolving

### Analyze with gomod

Project is [here](https://github.com/Helcaraxan/gomod).
Let's analyze [mainservice](https://github.com/godepsresolve/mainservice), run:
```
$ git clone git@github.com:godepsresolve/mainservice.git
$ cd mainservice
$ gomod graph -a '**' | dot -Tsvg -o graph.svg
```
Then open graph.svg with your favorite image viewer.
![gomod graph image](/assets/images/gomod_graph_versions.svg)

You can also run gomod only for your dependencies, because in large projects with many dependencies graph becomes unreadable:
`$ gomod graph -a 'github.com/godepsresolve/**' | dot -Tsvg -o graph.svg`.


### Analyze with modgraphviz

Project is [here](https://golang.org/x/exp/cmd/modgraphviz).
Let's analyze [mainservice](https://github.com/godepsresolve/mainservice), run:
```
$ git clone git@github.com:godepsresolve/mainservice.git
$ cd mainservice
$ go mod graph | modgraphviz | dot -Tsvg -o graph_modg.svg
```
Then open graph.svg with your favorite image viewer.
![modgraphviz graph image](/assets/images/modgraphviz_versions.svg)

As for only required group of dependency you can grep them before processing (space before github.com is not a typo):
`go mod graph | grep ' github.com' | modgraphviz | dot -Tsvg -o graph_modg.svg`.

## Analyze of dependency resolving with replaces

### Analyze with gomod

Let's analyze [mainservice_replace_fork](https://github.com/godepsresolve/mainservice_replace_fork), run:
```
$ git clone git@github.com:godepsresolve/mainservice_replace_fork.git
$ cd mainservice_replace_fork
$ gomod graph -a '**' | dot -Tsvg -o graph.svg
```
Then open graph.svg with your favorite image viewer.
![gomod graph image](/assets/images/gomod_graph_versions_replaced.svg)


### Analyze with modgraphviz

Let's analyze [mainservice_replace_fork](https://github.com/godepsresolve/mainservice_replace_fork), run:
```
$ git clone git@github.com:godepsresolve/mainservice_replace_fork.git
$ cd mainservice_replace_fork
$ go mod graph | modgraphviz | dot -Tsvg -o graph_modg.svg
```
Then open graph.svg with your favorite image viewer.
![modgraphviz graph image](/assets/images/modgraphviz_versions_replaced.svg)

As you can see modgrapviz method is useless here, because `go mod graph` does not consider replace directive. So I cannot recommend `go mod graph` with modgraphviz in case with replace.