.. Description of conjugate gradient squared module
.. _cgs-page:

=====================================
The Conjugate Gradient Squared Method
=====================================

The :mod:`cgs` Module
=====================

.. automodule:: cgs

.. autoclass:: CGS
   :show-inheritance:
   :members:
   :inherited-members:
   :undoc-members:


Example
=======

Here is an example using CGS on a linear system. The coefficient matrix is read
from file in Matrix Market format::

    import numpy as np
    from pykrylov.cgs import CGS as KSolver
    from pysparse import spmatrix
    from pysparse.pysparseMatrix import PysparseMatrix as sp

    A = sp(matrix=spmatrix.ll_mat_from_mtx('jpwh_991.mtx'))
    n = A.shape[0]
    e = np.ones(n)
    rhs = A*e

    ks = KSolver( lambda v: A*v,
                  matvec_max=2*n,
                  verbose=False,
                  outputStream=sys.stderr,
                  reltol = 1.0e-5 )
    ks.solve(rhs, guess = 1+np.arange(n, dtype=np.float))

    print 'Number of matvecs: ', ks.nMatvec
    print 'Initial/final res: %8.2e/%8.2e' % (ks.residNorm0, ks.residNorm)
    print 'Direct error: %8.2e' % (np.linalg.norm(ks.bestSolution-e)/sqrt(n))


Running this script produces the following output::

    Number of matvecs:  64
    Initial/final res: 8.64e+03/4.72e-03
    Direct error: 1.47e-04
