### In this lab, we will clean up the playbook from [test/vagrant3](https://github.com/Mohitsharma44/ansible-playbooks/tree/master/test/vagrant3) by introducing `include`.

`include` -- using this we can divide our playbook into multiple files and then call them from a single main yaml file.

`tag` -- using this we can only call the tasks that we want to run from command line e.g `ansible-playbook playbook.yml -i./hosts --tags setup_host` will only run the tasks that have `setup_host` tag.


For more info, refer [WiKi](https://github.com/Mohitsharma44/ansible-playbooks/wiki) page
