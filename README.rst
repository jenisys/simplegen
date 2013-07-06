simplegen
================================================================================

:License: BSD
:Requires: Python >= 2.5 (not: Python >= 3.0)
:Requires: Jinja2 >= 2.5 (license: BSD)

This project provides a simple code generator that uses a simple data model.
The data model can be defined via:

  * command-line define statements
  * JSON files
  * ...

`Jinja2`_ is used as powerful template engine for the templates.
This allows to use it for simple and more complex examples.

SEE ALSO:
  * http://jinja.pocoo.org/
  * http://www.json.org/

.. _Jinja2: http://jinja.pocoo.org/


EXAMPLE
-------------------------------------------------------------------------------

First you define the data model for this example.
It basically consists of a person database in the "persons.json" file.
The persons are just enumerated in an array/list.

.. code-block:: json

    {
        "persons": [
            { "name": "Alice",    "age": 25, "gender": "female" },
            { "name": "Bob",      "age": 31, "gender": "male"   },
            { "name": "Camille",  "age": 16, "gender": "female" }
        ]
    }

Then you provide a textual template file in Jinja2 syntax for your report.

.. code-block:: jinja

    # -- TEMPLATE-FILE: template.txt.in
    {%- set person = persons[person_id|int] %}

    person.id:   ${person_id}
    person.name: ${person.name}
    person.age:  ${person.age}

    Make me 10 years older: ${person.age + 10}

    __END-OF-FILE__

.. note::

    1. Placeholders use the ${placeholder} instead of {{placeholder}} syntax.
    2. The set-operation in the second line selects the person from the
       persons database by using the person_id as index into the array.
    3. To ensure that "person_id" is a number, its value is converted into
       an integer by using the "|int" Jinja filter expression.

Next you generate the report for Bob with "person_id=1" as index.
Simplegen writes to stdout because no --output option is used.

.. code-block:: bash

    $ simplegen --data=persons.json -D person_id=1 template.txt.in
    SIMPLEGEN: Loading persons.json ...
    SIMPLEGEN: Processing template.txt.in ...
    TEMPLATE-FILE: template.txt.in

    person.id:   1
    person.name: Bob
    person.age:  31

    Make me 10 years older: 41

    __END-OF-FILE__

