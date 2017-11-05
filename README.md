# wso2-puppet-modules-5x-upgrade

References :
* https://github.com/puppetlabs/puppet-in-docker-examples/tree/master/compose
* https://github.com/puppetlabs/puppet-in-docker

Steps to run :
1. Clone git repository 'wso2-puppet-modules-5x-upgrade'

    ```
    git clone https://github.com/DilanUA/wso2-puppet-modules-5x-upgrade.git
    ```
2. Create a mount directory called 'mnt' inside compose folder

    ```
    cd compose
    mkdir mnt
    ```
3. Copy 'jdk-8u131-linux-x64.tar.gz' in to compose/code/environments/production/modules/wso2base/files

4. Copy 'wso2am-2.1.0.zip' in to compose/code/environments/production/modules/wso2am_runtime/files

5. Build required docker images

    ```
    cd dockerfiles/puppet-agent-ubuntu
    docker build -t my-puppet-agent:1.0.0 .
    cd dockerfiles/puppet-server
    docker build -t my-puppet-server:1.0.0 .
    ```
    
6. Start puppet server

    ```
    cd compose
    docker-compose up
    ```

7. Run below command to up puppet agent

    ```
    docker run --volume <PATH-TO-GIT-REPO>/compose/mnt:/mnt 
    --env FACTER_product_name=wso2am_runtime 
    --env FACTER_product_version=2.1.0 
    --env FACTER_product_profile=default 
    --env FACTER_environment=production 
    --env FACTER_use_hieradata=true 
    --env FACTER_platform=default 
    --env FACTER_pattern=pattern-0 
    --env FACTER_vm_type=non-docker 
    --net compose_default my-puppet-agent:1.0.0
    ```
    