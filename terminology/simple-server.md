# The *simple-server*

<!-- TODO: Detailed specifications -->

```plantuml
@startuml
skinparam componentStyle uml2

interface HTTP
node Server {
    component "simple-server" as ss
    component "server-base" as sb
    component "client-base" as cb

    [ss] -- [cb] : serves >
    ss -- sb
}

node "External Modules" {
    component "Server Module" as sm1
    component "CBM" as cbm
    [sb]-- [sm1]: > use
    [cb]->[cbm]: > use
}

node "Authentication Provider" {
    database DB
    component "OAuth 2.0 Implementation" as oa
    interface sqlite

    [oa] -- sqlite
    [DB] <. sqlite : use
}

HTTP -- [ss]
ss .> oa : use
@enduml
```