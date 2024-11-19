# Interface
What should an interface look like for something like this? We essentially want to be able to define a set of LLM operations for each item in an iterable. There is also a question about how we should expect the data to be inputted. Should we expect the user to provide the items? Or should the agent use tooling to aggregate data?

I'd say that as far as scope is concerned, the former is the better option. The goal here is to provide a simple way to apply LM operations in parallel to a set of data, and while providing utilities to adapt a data source to the interface would be fine, it's outside the scope of this project to allow such a dynamic aggregation of such a data source.

So, let's break down the core components of the system which we will require the user to provide:

1. Aggregated data in a specified format
2. A set of instructions for the LM to apply to each item
3. A set of tools to allow the LM to apply them

In practice, this might look something like this:

```py
from parallm import ParaLLM, DirectorySource, Operation, Tool

source = DirectorySource(
    path="./data"
)

magic_number_instances: List[Tuple[str, str]] = []
def add_magic_number_instance(item_id: str, instance: str):
    append(magic_number_instances, (item_id, instance))

p = ParaLLM(
    source=source,
    operations=[
        Operation(
            name="Detect magic numbers",
            prompt="Detect instances of magic numbers",
            tool=Tool(
                name="report_instance",
                description="Report an instance of a magic number.",
                parameters={
                    "type": "object",
                    "properties": {
                        "instance": {
                            "type": "string",
                            "description": "The instance of a magic number that you've found.",
                        },
                    },
                    "required": ["instance"],
                },
                run=add_magic_number_instances
            )
        )
    ]
)

p.run()
```

Or something like that
