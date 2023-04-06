# API-first

!!! success "Guidance"
    - Providers **should** design their API before beginning implementation.


An API-first approach means that a team should design and iterate upon the API before beginning development and treat the API as a first-class product rather than just another component of the system. This approach:

  - Makes implementation go more smoothly
  - Reduces implementation time
  - Increases the changes implementation is correct by centering the API around the consumer
  - Does not lessen the importance of implementation

The resulting speed and accuracy benefits not only the providing team, but the consuming teams by allowing them to begin work earlier, and in parallel, when they have a design to build against. Mock response data can act as a placeholder in specs and tests while the provider fleshes out the implementation details.

Finally, developers often neglect documentation in software projects. A fortunate side effect of the API-first approach is that your design is also documentation. Documentation will be complete before you start implementation and will require only minor tweaks along the way to reflect your API when development is complete.

