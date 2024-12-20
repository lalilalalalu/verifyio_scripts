## Step 1: Conflict Detection
To detect conflicts, use the `conflict-detector` tool from Recorder:

```bash
$RECORDER_INSTALL_PATH/bin/conflict-detector /path/to/trace
```

Alternatively, the [`auto_detect.sh`](https://github.com/lalilalalalu/verifyio_scripts/blob/main/auto_detect.sh)  script can be used to process multiple traces and detect conflicts. The script requires the path to the directory containing the traces as an argument. The conflict files will be added to the corresponding directories after execution.

```bash
./auto_detect.sh /path/to/target/traces
```

## Step 2: Semantic Verification
The next step is to run the semantic verification using [`verifyio.py`](). By default, MPI-IO semantics and the vector clock algorithm are used for verification.

```bash
python /path/to/verifyio.py /path/to/trace
```
Available arguments:
* --semantics: Specifies the I/O semantics to verify. Choices are: POSIX, MPI-IO (default), Commit, Session, Custom
* --algorithm: Specifies the algorithm for verification. Choices are: 1: Graph reachability 2: Transitive closure 3: Vector clock (default) 4: On-the-fly MPI check
* --semantic_string: A custom semantic string for verification. Default is: "c1:+1[MPI_File_close, MPI_File_sync] & c2:-1[MPI_File_open, MPI_File_sync]""
* --show_details: Displays details of the conflicts.
* --show_summary: Displays a summary of the conflicts.
* --show_full_chain: Displays the full call chain of the conflicts.

Alternatively, the [`auto_verify.sh`](https://github.com/lalilalalalu/verifyio_scripts/blob/main/auto_verify.sh) script can be used for verification. This script requires the path to the directory containing the traces as an argument. By default, it verifies all supported semantics.

```bash
./auto_verify.sh /path/to/target/traces
```

## Step 3: Export Results to CSV

VerifyIO results can be exported to a CSV format for further analysis by using [`verifyio_to_csv.py `](https://github.com/lalilalalalu/verifyio_scripts/blob/main/verifyio_to_csv.py). Use the following command, providing the output file (usually stdout from VerifyIO execution) as an argument:

```bash
python verifyio_to_csv.py /path/to/verifyio/output /path/to/output.csv
```
Optional arguments:
* --dir_prefix="/prefix/to/remove": Removes a specified prefix from the paths in the output.
* --group_by_api: Groups the output by API calls.

## Step 4: Heatmap Visualization

To visualize the results from VerifyIO, use the [`verifyio_plot_heatmap.py`](https://github.com/lalilalalalu/verifyio_scripts/blob/main/verifyio_plot_violation_heatmap.py) script:

```bash
python verifyio_plot_heatmap.py --file=/path/to/output.csv
```
Alternatively, for visualizing up to three CSV files in a single heatmap, use the following argument:

```bash
python verifyio_plot_heatmap.py --files="/path/to/output1.csv" "/path/to/output2.csv" "/path/to/output3.csv"
```
