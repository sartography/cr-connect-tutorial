.. _index-scripts:

=================
Creating a Script
=================

`Scripts` can be called from a script task in workflows. They are a great way to extend workflow capabilities.

Scripts extend the `Script` base class found in crc.scripts.script, and they must define the
`get_description`, `do_task`, and `do_task_validate_only` methods.

The get_description method should return a string that is displayed when configurators get a list of available scripts.

The do_task_validate_only method is run during the configurator `shield test`.
We only need to make sure that the script *can* run here. We don't want to make any externals calls.

The do_task method is where we put the code we need to achieve our task.

.. toctree::
   :maxdepth: 2

   01_script
   02_workflow
   03_running
