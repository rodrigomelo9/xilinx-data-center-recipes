# Amazon FPGA Workflow

## Host and FPGA binaries building

### Software defined development with Vitis (Makefile based)

Preparation:
```bash
source $AWS_FPGA_REPO_DIR/vitis_setup.sh
cd <YOUR_PROJECT_DIR>
```

Software Emulation:
```bash
make clean
make check TARGET=sw_emu DEVICE=$AWS_PLATFORM all
```

Hardware Emulation:
```bash
make clean
make check TARGET=hw_emu DEVICE=$AWS_PLATFORM all
```

Build the binaries:
```bash
make clean
make TARGET=hw DEVICE=$AWS_PLATFORM all
```

> **Note:** to keep a processes running after a SSH session disconnection, you can use:
> ```bash
> nohup long-running-process &
> exit
> ```

### Software defined development with Vitis (GUI based)

[TODO](https://github.com/Xilinx/SDAccel-Tutorials/blob/master/docs/aws-getting-started/CPP/STEP2.md)

### Hardware development with Vivado (Makefile based)

Preparation:
```bash
source $AWS_FPGA_REPO_DIR/hdk_setup.sh
cd <YOUR_PROJECT_DIR>
export CL_DIR=$(pwd)
```

Build the Custom Logic (CL):
```bash
cd $CL_DIR/build/scripts
./aws_build_dcp_from_cl.sh
```

> Optionally, to be notified via e-mail when the build completes
> ```bash
> pip install --user --upgrade boto3 # If not previously installed
> export EMAIL=your.email@example.com
> $AWS_FPGA_REPO_DIR/shared/bin/scripts/notify_via_sns.py
> ```
> Check your e-mail and confirm subscription (first time).
> ```bash
> ./aws_build_dcp_from_cl.sh -notify
> ```

Build the host binary:
```bash
cd $CL_DIR/software/runtime/
make all
```

## AFI creation

> Here is assumed that your bucket was already created and called `your-bucket-for-afis`.
> Also, the DCP and LOGS directory were created, and called `dcp` and `logs` respectively.
> More info [here](aws-configs.md#creation-of-a-s3-bucket-and-folders).

**Software defined flow:**
```bash
$AWS_FPGA_REPO_DIR/Vitis/tools/create_vitis_afi.sh -xclbin=build_dir.hw.<AWS_PLATFORM>/<FILENAME>.xclbin -s3_bucket=your-bucket-for-afis -s3_dcp_key=dcp -s3_logs_key=logs
cat <TIMESTAMP>_afi_id.txt
```

**Hardware defined flow:**
```bash
aws s3 cp $CL_DIR/build/checkpoints/to_aws/*.Developer_CL.tar s3://your-bucket-for-afis/dcp/
aws ec2 create-fpga-image --region us-west-2 --name hello-hdl --description hello-hdk --input-storage-location Bucket=your-bucket-for-afis,Key=dcp/<DATE>-<TIME>.Developer_CL.tar --logs-storage-location Bucket=your-bucket-for-afis,Key=logs
```

Wait until the AFI has been created successfully (`"Code": "available"`), running:
```bash
aws ec2 describe-fpga-images --fpga-image-ids <AFI_ID>
```

> Optionally, to be notified via e-mail when the build completes
> ```bash
> wait_for_afi.py --afi <AFI_ID> --notify --email <YOUR_EMAIL_HERE>
> ```
> Check your e-mail and confirm subscription (first time).

## Hardware Execution

> Here is assumed that you performed development on another instance (without FPGA), and you copied the `<HOSTBIN>`, `<FPGABIN>` and the required data files to a well configured and running FPGA instance, where the `aws_fpga` repository was cloned.

Preparation:
```bash
source $AWS_FPGA_REPO_DIR/vitis_runtime_setup.sh
```

**Software defined flow:**
```bash
./<HOSTBIN> <FPGABIN>.awsxclbin
```

**Hardware defined flow:**
```bash
fpga-clear-local-image -S 0
fpga-load-local-image -S 0 -I agfi-xxxxxxxxxxxxxxxxx
./<HOSTBIN>
```
