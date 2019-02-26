# External Hash Tree on Blockchain Principle
## Include Hash Root Info (HRI) in a blockchain transaction in a form of TaggedData:

    <TaggedData[2+]>
    TaggedData[2+] = <RootTag[1(byte)]><VersionTag[1]><DataSet[0+]>[<SignatureTag[1+]>]
    RootTag[1] = 0x12 - signals that the OP_RETURN will contain Tagged Data 12 (TD12)
    VersionTag[1] - indicates the version of TD12 format:
      0x01 - current version
      0xFF - custom version depend on use case, 
      Other values - RFU
    DataSet[0+] = [<DataItem1[1+]>[<DataItem2[1+]>[<DataItem3[1+]>...[<DataItemN[1+]>]...]]]
      DataItem[1+] - stored data in T[L]V (Tag[1] [Length[1]] Value[0..Length-1]) format:
        Tags:
          0x00 - RFU
          0x01 - SHA1 hash (Length = 20)
          0x02 - SHA2-256 hash (Length = 32)
          0x03 - SHA3-256 hash (Length = 32)
          0x04 - 2xSHA2-256 hash (Length = 32) - double hashing with SHA2-256
          0x05 - MD5 hash (Length = 16)
          0x06 - BTIH hash (Length = 20) - BitTorrent Info Hash used in Torrent P2P networks to identify files and create links
          0x07 - TTF hash (Length = 24) - Tiger Tree Hash based on used in Direct Connect P2P networks to identify files and create magnet links
          0x08 - ED2K hash (Length = 16) - eDondey2000 hashing based on MD4 used in eMule P2P networks to identify files and create ed2k links
          0x09 - CRC32 (Length = 4) - Cyclic Redundancy Check
          0x0A - RIPEMD160 hash (Length = 20)
          0x0B - BLAKE2s hash (Length = 32)
          0x0C - RFU
          .	 - RFU
          .	 - RFU
          0xFD - RFU
          0xFE - reserved for SignatureTag
          0xFF - custom tag, depend on use case (e.g. when need to store encrypted file hash)
        Length[1]: 
          Absent - for tags 0x01..0x08
          Depent on use case - for tag 0xFF
          Other values - RFU
        Value[0..Length-1]:
          MSB first
    SignatureTag[1+] - stored data signature in T[L]V (Tag[1] [Length[1]] Value[0..Length-1]) format:
      Tags:
        0xFE - signature by shortened hashing:
          Value = Right(2xSHA256(x), hashLength bytes)
            x = <TaggedData[2+]><Salt[0+]>
              Salt[0+] - application-specific secret data
        Other Tags are RFU
      Length[1]: 
        For Tag 0xFE: hashLength[1]
      Value:
        MSB first


## For Bitcoin:

To store additional bytes in a transaction or/and for other needs you can customize transaction destination address (e.g. use BitTorrent Info Hash).

Store HRI in transaction OP_RETURN (40 bytes of data max).


## For Ethereum:

To store additional bytes in a transaction or/and for other needs you can customize transaction destination address (e.g. use BitTorrent Info Hash).

Store HRI in a transaction data field or/and in a contract.
