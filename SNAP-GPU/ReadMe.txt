I've attached the input files for running the HfO simulation:
SimScale.in 			- LAMMPS input file
coeff.31.snapcoeff		- Snap coefficient file for model using 31 coefficients (used by SimScale.in)
param.31.snapparam		- Snap prameter file for model using 31 coefficients (used by SimScale.in)
log.lammps				- Record of simulation progress, thermodynamic data, and any error messages
xyz.Pos_data.lammpstrj	- Record of atom coordinates. Can be visualized using VMD (Visual Molecular Dynamics)

SimScale.in runs a constant volume simulation of argon atoms (at a number density equal to that of solid hafnium) at a constant temperature (300K by default) and constant volume. The number of molecules in the simulation can be set using the xUnitCells,yUnitCells, and zUnitCells variables in the "Set Common Simulation Parameters" section of the input file. We are still working on generating other SNAP models using different numbers of coefficients. Swithcinng between the current 31 coefficient model and future models can be done using the "NumberSnapCoeffs" variable.





The options that we generally use when installing LAMMPS are covered in LAMMPS-install.txt
Basic makefiles that enable OpenMP and KOKKOS (GPU or OMP) can be found in /lammps.../src/MAKE/OPTIONS/ 


LAMMPS simulations can be run using OpenMPI, OpenMPI, KOKKOS, or any combination of these. However the COMB potential has not been implemented in KOKKOS.


To run LAMMPS just using OpenMPI:
	mpirun -np (#MPInodes) /.../lmp_mpi <SimScale.in


To run LAMMPS using OpenMPI and OpenMP:
	mpirun -np (#MPInodes) /.../lmp_omp -sf omp -pk omp (#ThreadsPerNode) <SimScale.in

To run LAMMPS using OpenMPI and KOKKOS(GPU):
	mpirun -np (#MPInodes) /.../lmp_kokkos_cuda_mpi -k on g (#Max_GPUs_Per_MPInode) -sf kk -pk kokkos newton on neigh half <SimScale.in

To run LAMMPS using OpenMPI and KOKKOS(OpenMP):
	mpirun -np (#MPInodes) /.../lmp_kokkos_omp -k on t (#OMP_threads_per_MPInode) -sf kk <SimScale.in
