## How to link 6xx Marc fields, subdivisions in subfields x,y,z to authority records. This allows the use of unpreferred terms from authority records for searching by users, not only the terms directly included in x,y,z subfields.

The authority records contain authority ID (subfield 7 or field 001) that are used for linking to BIB 6xx fields in general. But subfields x,y,z do not contain the ID, but as big most of them are unique terms, they can be linked to AUT. Like in the case of National Authorities of the Czech Republic.

### Solution:
For this example, we use BIB01 as bibliographic base and AUT10 as authority base.

1. Put the terms from AUT10 to GEN acc heading also without the IDs: 
.../aut10/tab/tab11_acc

     `15###                    GEN   a`

and reindex the base (create new headings)


2. In bib base, set new acc heading ZYX. Create on ACC expanded field ZYX from subfields x,y,z and insert the values into it the headings ZYX
...bib01/tab/tab01.lng

    ` H ZYX   ACC     12 00       00       Subject subdivisions xyz`

...bib01/tab/tab_expand

    ACC        fix_doc_do_file_08             acc_zyx.fix

...bib01/tab/import/acc_zyx.fix  (new file)

    !   2   3  4  5   6    7                 8                           9
`!-!!!!!-!!-!-!!!-!!!-!!!!!-!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!-!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!`
  
    1 65###                    COPY-FIELD                     ZYX X,L
    1 65###                    COPY-FIELD                     ZYX Y,L
    1 65###                    COPY-FIELD                     ZYX Z,L
    1 ZYX X                    DELETE-SUBFIELD                a
    1 ZYX X                    DELETE-SUBFIELD                v
    1 ZYX X                    DELETE-SUBFIELD                y
    1 ZYX X                    DELETE-SUBFIELD                z
    1 ZYX X                    DELETE-SUBFIELD                2
    1 ZYX X                    DELETE-SUBFIELD                7
    1 ZYX X                    REPLACE-STRING                 $$x,$$a
    !
    1 ZYX Y                    DELETE-SUBFIELD                a
    1 ZYX Y                    DELETE-SUBFIELD                v
    1 ZYX Y                    DELETE-SUBFIELD                x
    1 ZYX Y                    DELETE-SUBFIELD                z
    1 ZYX Y                    DELETE-SUBFIELD                2
    1 ZYX Y                    DELETE-SUBFIELD                7
    1 ZYX Y                    REPLACE-STRING                 $$y,$$a
    !
    1 ZYX Z                    DELETE-SUBFIELD                a
    1 ZYX Z                    DELETE-SUBFIELD                v
    1 ZYX Z                    DELETE-SUBFIELD                x
    1 ZYX Z                    DELETE-SUBFIELD                y
    1 ZYX Z                    DELETE-SUBFIELD                2
    1 ZYX Z                    DELETE-SUBFIELD                7
    1 ZYX Z                    REPLACE-STRING                 $$z,$$a


...bib01/tab/tab00.lng

    D ZYX## 12 00 0000   ZYX   ZYX   LZYX

...bib01/tab/tab11_acc

    ZYX##                    ZYX


3. set the linking between bib ZYX and aut GEN


...bib01/tab/tab20
_(A common heading for subject headings is SUB here. Note that settings (lines) linking to COR## in authoritz base may be probably ommited. The linking is made on virtual expanded fields ZYX, not real 6xx Marc fields. Thus changes in authorities using COR fields/mechanism will not cause any changes in 6xx bibliographic fields.)_

    1 ZYX   SUB   15###                                   0
    2             45###              -wi                  0 SEEF
    1 ZYX   SUB   15###                                   0
    2             COR##              -wi                  0 SEEF
    1 ZYX   SUB   15###                                   0
    2             75###              -wi                  0 SEEF
    1 ZYX   ZYX   15###              -27                  0
    2             45###              -wi                  0 SEEF
    !1 ZYX   ZYX   15###                                   0
    !2             COR##              -wi                  0 SEEF
    1 ZYX   ZYX   15###              -27                  0
    2             75###              -wi                  0 SEEF
    1 ZYX   SUB   100##                                   0
    2             400##              -wi                  0 SEEF
    1 ZYX   SUB   100##                                   0
    2             COR##              -wi                  0 SEEF
    1 ZYX   SUB   100##                                   0
    2             700##              -wi                  0 SEEF
    1 ZYX   ZYX   100##                                   0
    2             400##              -wi                  0 SEEF
    1 ZYX   ZYX   100##                                   0
    2             COR##              -wi                  0 SEEF
    1 ZYX   ZYX   100##                                   0
    2             700##              -wi                  0 SEEF
    1 ZYX   SUB   11###                                   0
    2             41###              -wi                  0 SEEF
    1 ZYX   SUB   11###                                   0
    2             COR##              -wi                  0 SEEF

...bib01/tab/tab_aut

    ZYX     AUT10


4. Add expanded ZYX fields to word index(es) that will contain the unpreferred terms from authority base as well. Expand the ZYX field on WORd expansion.

.../bib01/tab11_word
_(WRD is general index "all words" here, WKW is index fro subject headings; modificate this according your settings of indexes)_

    ZYX##                    avxyz                03     WRD  WKW

.../bib01/tab_expand
    WORD       fix_doc_do_file_08             acc_zyx.fix


5.  Reindex BIB01 records with 6xx fields containing x,y,z subfields.


6. Check the settings on some record that you reindex. For relinking all headings in ZYX heading use sql:

    update bib01.z01 set z01_rec_key_4='-NEW-000000000' where z01_rec_key_4='-CHK-000000000' and z01_rec_key like 'ZYX%';











