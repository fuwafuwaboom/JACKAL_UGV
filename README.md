
## Install ACACOS

following this [tutorial](https://docs.acados.org/installation/index.html) (choose **CMake** part to proceed, not **Make**) to install

If your Ubuntu is running on WSL, follow the **Windows 10+(WSL)** part first

## Running Example Interface

There are 3 interfaces of ACADOS:

1. [C Interface](https://docs.acados.org/c_interface/index.html)
2. [Python Interface](https://docs.acados.org/python_interface/index.html)
3. [Matlab + Simulink and Octave interface](https://docs.acados.org/matlab_octave_interface/index.html)

I choose Python Interface (Currently, Python >= 3.8 is tested).

1. Make sure you compile and install `acados` by following the [CMake installation instructions](https://docs.acados.org/installation/index.html).
2. In one directory you choose, create a Python virtual environment
    
    ```bash
    virtualenv env --python=/usr/bin/python3
    ```
    
3. activate this environment
    
    ```bash
    source env/bin/activate
    ```
    
4. Install `acados_template` Python package (replace the <acados_root> with the path to your acados ):
    
    ```bash
    pip install -e <acados_root>/interfaces/acados_template
    ```
    
5. Add the path to the compiled shared libraries `libacados.so, libblasfeo.so, libhpipm.so` to `LD_LIBRARY_PATH` (default path is `<acados_root/lib>`) by running:
    
    ```bash
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:"<acados_root>/lib"
    export ACADOS_SOURCE_DIR="<acados_root>"
    ```
    
6. Now (2024/6/25) ACADOS has a bug with the HPIPM & BLASFEO targets, so do the following to fix it:
    
    ```bash
    cd <acados_dir>
    rm build/* -rf
    cd build
    cmake .. -DACADOS_WITH_QPOASES=ON -DACADOS_EXAMPLES=ON -DHPIPM_TARGET=GENERIC -DBLASFEO_TARGET=GENERIC
    make -j4
    make install -j4
    # run a C example, e.g.:
    ./examples/c/sim_wt_model_nx6
    ```
    
    if you can successfully run a C example, meaning you has fixed this bug
    
7. Now run a Python example:
    
    ```bash
    cd <acados_dir>
    cd examples/acados_python/getting_started/
    python3 minimal_example_ocp.py
    ```
    
    you will see bug: `! LaTeX Error: File 'type1cm.sty' not found.`
    
    Now go into this `minimal_example_ocp.py` file, modify `latexify=True`  into `latexify=False`
    
    Then run it again:
    
    ```bash
    python3 minimal_example_ocp.py
    ```
    
    You should see the result like the following:
    
    ![Untitled](https://github.com/wisc-arclab/JACKAL_UGV/blob/ACADOS_NMPC_ROS/image.png)
    
    Means you have successfully installed ACAODS
