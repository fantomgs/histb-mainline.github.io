SoCs
====

Chip ID Table
-------------

==============  ==============  ==============  ===============
SoC             Chip ID         Mask [#]_       Link
==============  ==============  ==============  ===============
Hi3798CV100A    ``0019050100``  ``FFFFFFFFFF``
Hi3796CV100     ``1819050100``  ``FFFFFFFFFF``
Hi3798CV100     ``1C19050100``  ``FFFFFFFFFF``
Hi3716CV200ES   ``0019400200``  ``FFFFFFFFFF``
Hi3716MV400     ``1C37160200``  ``FEFFFFFFFF``
Hi3716CV200     ``0037160200``  ``FFFFFFFFFF``
Hi3716HV200     ``0437160200``  ?
Hi3719CV100     ``0837160200``  ?
Hi3718CV100     ``1037160200``  ?
Hi3716MV400     ``1C37160200``  ``FEFFFFFFFF``
Hi3719MV100A    ``1E37160200``  ``FEFFFFFFFF``
Hi3716MV420     ``0037160410``  ``FFFFFFFFFF``
Hi3716MV410     ``0137160410``  ``FFFFFFFFFF``
Hi3716MV430     ``0037160430``  ``00FFFFFFFF``  [Hi3716MV430]_
Hi3716MV420B03  ``0037160450``  ``FFFFFFFFFF``
Hi3716MV420N    ``0037160460``  ``FFFFFFFFFF``
Hi3716MV410N    ``0137160460``  ``FFFFFFFFFF``
Hi3719MV100     ``0037190100``  ``FEFFFFFFFF``
Hi3718MV100     ``0437190100``  ``FEFFFFFFFF``
Hi3796MV200     ``0037960200``  ``00FFFFFFFF``  [Hi3796MV200]_
Hi3716MV450     ``1037960200``  ``FEFFFFFFFF``
Hi3796MV100     ``0037980100``  ``F1FFFFFF0F``
Hi3798MV100     ``0137980100``  ``F1FFFFFF0F``
Hi3716DV100     ``1037980100``  ``F1FFFFFF0F``
Hi3798CV200     ``0037980200``  ``FFFFFFFFFF``  [Hi3798CV200]_
Hi3798MV200     ``0037986200``  ``FFFFFFFFFF``  [Hi3798MV200]_
Hi3798MV200_A   ``0037988200``  ``FFFFFFFFFF``
Hi3798MV300     ``0037980210``  ``FFFFFFFFFF``
Hi3798MV310     ``0037980300``  ``FFFFFFFFFF``  [Hi3798MV310]_
Hi3798MV300H    ``0137980300``  ``FFFFFFFFFF``
Hi3798MV200H    ``0237980300``  ``FFFFFFFFFF``  [Hi3798MV200H]_
Hi3796CV300     ?               ?               [Hi3796CV300]_
==============  ==============  ==============  ===============

Notes:

.. [#] Chip IDs and Masks are recorded as is. They could be wrong (especially Masks); don't trust them blindly.

Links:

.. [Hi3716MV430] https://www.hisilicon.com/products/smart-media/STB/Hi3716MV430C-S-T
.. [Hi3796CV300] https://www.hisilicon.com/products/smart-media/STB/Hi3796CV300
.. [Hi3796MV200] https://www.hisilicon.com/products/smart-media/STB/Hi3796MV200
.. [Hi3798CV200] https://www.hisilicon.com/products/smart-media/STB/Hi3798CV200
.. [Hi3798MV200] https://www.hisilicon.com/products/smart-media/STB/Hi3798MV200
.. [Hi3798MV200H] https://www.hisilicon.com/products/smart-media/STB/Hi3798MV200H
.. [Hi3798MV310] https://www.hisilicon.com/products/smart-media/STB/Hi3798MV310

HiSilicon will remove descriptions of disconnected chips completely from their website. Don't be surprised if any links are broken.

See alse Archive `1 <https://web.archive.org/web/*/https://www.hisilicon.com/en/products/smart-media/STB*>`_ and `2 <https://web.archive.org/web/*/https://www.hisilicon.com/products/smart-media/STB*>`_.

Chip ID Meaning
---------------

- ``[37:32]``: ``CHIP_ID`` from ``SOC_FUSE_0[20:16]``, indicates die package
- ``[31:0]``: ``SC_SYSID`` from ``SC_SYSID``, indicates SoC version

  The following fields are inferred:

  - ``[31:16]``: SoC series
  - ``[15:12]``: variant
  - ``[11:8]``: major version
  - ``[7:4]``: minor version
  - ``[3:0]``: ?

37960200
~~~~~~~~~~

=======  =========
CHIP_ID   Meaning
=======  =========
``0``    BGA 21x21
``1``    BGA 15x15
=======  =========

37980100
~~~~~~~~~~

=======  =========
CHIP_ID   Meaning
=======  =========
``0``    BGA 23x23
``1``    BGA 19x19
``3``    BGA 15x15
``7``    QFP 216
``10``   BGA 21x21
=======  =========

Read Chip ID
------------

.. code-block:: c

  #define SYS_CTRL    0xf8000000
  #define  SC_SYSID    0xee0
  #define PERI_CTRL   0xf8a20000
  #define  SOC_FUSE_0  0x840
  #define   CHIP_ID     GENMASK(20, 16)

  void __iomem *sysctrl;
  void __iomem *perictrl;
  u32 sysid;
  u32 fuse0;

  sysctrl = ioremap(REG_BASE_SCTL, 0x1000);
  perictrl = ioremap(REG_BASE_PERI_CTRL, 0x1000);

  sysid = readl_relaxed(sysctrl + SC_SYSID);
  fuse0 = readl_relaxed(perictrl + SOC_FUSE_0);

  return ((u64) ((fuse0 & CHIP_ID) >> 16) << 32) | sysid;
