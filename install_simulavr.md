From https://www.klipper3d.org/Debugging.html#testing-with-simulavr

```
git clone git://git.savannah.nongnu.org/simulavr.git
cd simulavr
./bootstrap
./configure --enable-python
make
```

but before bootstramp command, as described on http://savannah.nongnu.org/bugs/?49033
```
Remove/comment out the line:
 GTEST_CXXFLAGS = -Dprivate=public -Dprotected=public
from Makefile.am in regress/gtest, reconfigure and it should compile fine.
```

and

```
apt install swig
pip3 install sphinx
```
