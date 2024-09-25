---
title: "switch between contexts to work on different clusters"
seoTitle: "switch between contexts to work on different clusters"
datePublished: Mon Apr 01 2024 18:03:43 GMT+0000 (Coordinated Universal Time)
cuid: cluh9e9rz000508jy0wla5ljk
slug: switch-between-contexts-to-work-on-different-clusters
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711994440456/df98e544-46e8-4b10-88d4-4e136dcb6b9b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1711994563898/fe81ce51-4908-40c5-959c-623ab97107bd.png
tags: kubernetes, beginner, devops, kubernetes-container, kubernetes-architecture, containerorchestration

---

### **Kubectl Context Alias**

If you regularly switch between contexts to work on different clusters, you can create an alias to list and set the context.

Here is the alias to list the contexts.

```plaintext
alias klist='kubectl config get-contexts -o=name'
```

You can add the following line to your `.bashrc` or `.zshrc` file, depending on the shell you use:

Create the following alias to set the context with a custom namespace.

```plaintext
kset() {
    local context="$1"
    local namespace="$2"
    kubectl config use-context "$context" --namespace "$namespace"
}
```

Here is how the aliases look in your `bashrc` or `zshrc` file.

```plaintext
alias k=kubectl

alias klist='kubectl config get-contexts -o=name'

kset() {
    local context="$1"
    local namespace="$2"
    kubectl config use-context "$context" --namespace "$namespace"
}

kdel() {
    kubectl config delete-context "$1"
}
```

Here is how you can use these aliases

1. List the contexts with **klist** command.
    
2. Set the context and namespace using the **kset** command