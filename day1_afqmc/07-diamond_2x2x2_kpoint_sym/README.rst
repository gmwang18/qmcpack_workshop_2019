Example 7: 2x2x2 Diamond k-point symmetry
==========================================

In this example we will show how to run an AFQMC simulation that exploits k-point symmetry
which is much more efficient that running in the supercell way discussed in the previous
example. We will again look at the same 2x2x2 cell of diamond. We assume you have run the
scf calculation in the previous example.

Essentially all that changes in the integral generation step is that we pass the
`-k/--kpoint` flag to `pyscf_to_afqmc.py`.

.. code-block:: bash

    mpirun -n 8 /path/to/qmcpack/utils/afqmctools/bin/pyscf_to_afqmc.py -i ../06-diamond_2x2x2_supercell/scf.chk -o hamil.h5 -t 1e-5 -v -a -k -q afqmc.xml

You will notice that now the Cholesky decomposition is done for each momentum transfer
independently and the the form of the hamiltonian file has changed to be k-point dependent.

In addition the input file `afqmc.xml` contains a different Hamiltonian xml block

.. code-block:: xml

        <Hamiltonian name="ham0" type="KPFactorized" info="info0">
          <parameter name="filetype">hdf5</parameter>
          <parameter name="filename">hamil.h5</parameter>
        </Hamiltonian>

where we see that the Hamiltonian type has changed from `Factorized` to `KPFactorized`.

Apart from these changes, running the AFQMC simulation proceeds as before, however you
should see a significant performance boost relative to the supercell simulations,
particularly on GPU machines.
