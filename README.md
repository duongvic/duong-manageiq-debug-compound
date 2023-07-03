# ManageIQ Debug Compound

Contains RubyMine debug configurations for core ManageIQ (w/ OpenStack plugin) workers.

This compound helps avoid running the whole EVM suite via `rake evm:start` wherever possible, which makes startup much faster.

More workers will be added as per debug necessities.

## How to use

Clone into manageiq/plugins and the debug configurations should be loaded automatically.

You can either debug individual workers or debug the compound "ManageIQ Workers" to start all workers simultaneously.

Simply stop all worker processes and restart the debug compound when you make changes to your code.

**NOTE:** If you get the `server roles` error, try the following steps:
1. Run `rake:evm start` once to apply server roles to the workers
2. Start the worker compound in debug mode and wait for the MIQ server to kill them off
3. Run `rake:evm kill` to kill all MIQ processes
4. Run the compound in debug mode again.

**NOTE 2:** If you get the `server starting` error when logging in as a non-superadmin user, replace `MiqUiWorker` and `MiqEventHandler` with `Development: manageiq` in the compound.

## Running EMS Workers
The debug console does not recognize the `--ems-id` option, so we can only *run* them.

In our case, the only EMS we'll be working at for now is OpenStack (OpS).
#### Get EMS ID from PGAdmin4:
1. Open PGAdmin4
2. Choose the `vmdb_development` database
3. Right click on it, choose **Query Tool** and enter
```
SELECT id, name, type FROM ext_management_systems
```
#### Apply EMS ID
1. Go to **Edit Configurations...** and select each OpS configuration
2. Replace `--ems-id <id>` in *Script arguments* with corresponding value from the `id` column
3. **Run** the **`OpenStack Workers`** compound.

## Effectively watching your worker processes while debugging
1. Right click the terminal in RubyMine
2. Select `Split Right`
3. In that new window on the right, enter the command:

Linux OS distributions:
```
watch 'ps -aux | grep MIQ'
```
MacOS:
```
watch 'ps | grep MIQ'
```