A trust is used to establish forest-forest or domain-domain authentication.
* `Parent-child` - Two or more domains with in the same forest. Child domain has a two-way transitive trust with the parent domain. (corp.domain.local and domain.local).
* `Cross-link` - A trust between child domains to speed up authentication.
* `External` - A non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust. Uses SID filtering.
* `Tree-root` - A two-way transitive trust between a forest root domain and a new tree root domain. They are created by design when you set up a new tree root domain within a forest.
* `Forest` - A transitive trust between two forest root domains.
* `ESAE` - A bastion forest used to manage Active Directory.
* ![[Pasted image 20260731095503.png]]
#### Trust Table Side By Side

| Transitive                                                            | Non-Transitive                              |
| --------------------------------------------------------------------- | ------------------------------------------- |
| Shared, 1 to many                                                     | Direct trust                                |
| The trust is shared with anyone in the forest                         | Not extended to next level child domains    |
| Forest, tree-root, parent-child, and cross-link trusts are transitive | Typical for external or custom trust setups |
![[Pasted image 20260731100230.png]]
