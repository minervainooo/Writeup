# Sniffed Off the Wire

https://squarectf.com/challenges/sniffed-off-the-wire

## Discription

After weeks of perching, our avian operatives captured a suspicious network flow. Maybe there's valuable data inside?
https://cdn.squarectf.com/challenges/sniffed-off-the-wire.pcap

## Solution

Opening the pcap with wireshark, there were about 40000 packets.
These packets could be export into a txt file  ( Wireshark > File > Export Packet Dissections )

```
0000  1b 5b 33 3b 32 30 48 74                           .[3;20Ht
0000  1b 5b 32 34 3b 32 34 48 64                        .[24;24Hd
0000  1b 5b 32 30 3b 31 38 48 57                        .[20;18HW
0000  1b 5b 31 35 3b 33 37 48 46                        .[15;37HF
0000  1b 5b 36 3b 35 35 48 72                           .[6;55Hr
0000  1b 5b 31 34 3b 32 35 48 4c                        .[14;25HL
0000  1b 5b 31 36 3b 37 33 48 63                        .[16;73Hc
0000  1b 5b 31 37 3b 32 37 48 45                        .[17;27HE
0000  1b 5b 31 32 3b 31 31 48 53                        .[12;11HS
0000  1b 5b 39 3b 37 36 48 49                           .[9;76HI
0000  1b 5b 30 3b 34 33 48 53                           .[0;43HS
0000  1b 5b 31 3b 36 31 48 44                           .[1;61HD
0000  1b 5b 32 30 3b 34 31 48 50                        .[20;41HP
0000  1b 5b 32 33 3b 33 37 48 4c                        .[23;37HL
0000  1b 5b 35 3b 37 39 48 55                           .[5;79HU
0000  1b 5b 30 3b 36 33 48 42                           .[0;63HB
0000  1b 5b 31 33 3b 33 37 48 6c                        .[13;37Hl
0000  1b 5b 31 33 3b 31 31 48 68                        .[13;11Hh
0000  1b 5b 33 3b 32 38 48 78                           .[3;28Hx
0000  1b 5b 31 31 3b 33 36 48 76                        .[11;36Hv
0000  1b 5b 31 35 3b 34 35 48 47                        .[15;45HG
0000  1b 5b 31 36 3b 35 37 48 75                        .[16;57Hu
0000  1b 5b 31 34 3b 33 34 48 69                        .[14;34Hi
0000  1b 5b 32 32 3b 36 33 48 76                        .[22;63Hv
0000  1b 5b 31 37 3b 32 31 48 74                        .[17;21Ht
0000  1b 5b 32 31 3b 33 31 48 6a                        .[21;31Hj
0000  1b 5b 31 36 3b 32 48 49                           .[16;2HI
0000  1b 5b 31 35 3b 36 48 64                           .[15;6Hd
0000  1b 5b 31 30 3b 37 34 48 7a                        .[10;74Hz
0000  1b 5b 31 37 3b 33 32 48 49                        .[17;32HI
0000  1b 5b 35 3b 34 35 48 46                           .[5;45HF
0000  1b 5b 32 31 3b 34 34 48 46                        .[21;44HF
0000  1b 5b 31 36 3b 31 33 48 70                        .[16;13Hp
0000  1b 5b 31 37 3b 33 30 48 79                        .[17;30Hy
0000  1b 5b 37 3b 34 48 5a                              .[7;4HZ
0000  1b 5b 31 37 3b 35 39 48 62                        .[17;59Hb
0000  1b 5b 30 3b 36 32 48 70                           .[0;62Hp
0000  1b 5b 32 30 3b 36 32 48 48                        .[20;62HH
0000  1b 5b 31 32 3b 37 36 48 69                        .[12;76Hi
0000  1b 5b 31 36 3b 32 34 48 51                        .[16;24HQ
0000  1b 5b 32 30 3b 32 37 48 77                        .[20;27Hw
0000  1b 5b 32 33 3b 33 38 48 67                        .[23;38Hg
0000  1b 5b 31 38 3b 32 32 48 4b                        .[18;22HK
0000  1b 5b 31 30 3b 35 34 48 50                        .[10;54HP
0000  1b 5b 32 3b 34 30 48 72                           .[2;40Hr
0000  1b 5b 30 3b 38 48 6f                              .[0;8Ho
0000  1b 5b 31 34 3b 36 36 48 69                        .[14;66Hi
0000  1b 5b 30 3b 37 30 48 7a                           .[0;70Hz
0000  1b 5b 31 37 3b 36 36 48 57                        .[17;66HW
```

All of them were represented as ```.[number ; number H char```
I guessed the pair number should be the coordination of an 2D array
then I wrote a python code to observe these data

With some simple observation, we can find the size of array is $25\cdot80$
yet the quantity of the packets is much greater then $25\cdot80 = 2000$

The array should be many layers, i.e. this is a 3D array 

```python
import re

M = [[ list() for x in range(100)] for y in range(100)] 
N = [[ list() for x in range(100)] for y in range(100)]

f = open("pcap")

for line in f:
    a += 1
    L = []
    line = line[line.find(".[")+2:]
    L += re.split('[;H]',line)[:2]
    M[int(L[0])][int(L[1])] .append( line[len(line)-2] )
   

for x in range(0,10):
    for i in range(0,25):
        for j in range(0,80):
            if M[i][j][x] :
                print(M[i][j][x],end='')  
        print('')
    print('')

```

than the flag is in one of the layers

```
fPAEuBvzuYVfemoqMtCgrMnEYBRcOoZLhqSUjzoSxgQarZPladIvPIItNmPsVhTzDyTcppfZarkBufoO
AcAtolqYHrkqDRZXsAkNfblYXdLAcrohtDrxIauCYdYwtBRIIYJOFCvLoGwKTtwtqSqXqHtXBPUYJysJ
ToKwgJUTppCNLSBoFWshIUrOnZKvJPCSleobtpWRAvSgVwJXKDptWgRilQfPFsGkpdoPiUvCuPzTkiXa
UUnuWXneUUuNypdepVNoUcZcgLWZOhMUZxsLhkpfJBXmqAwZcbPVHzOYHxAVcdHioLSaoVHTtaoJSzJq
aeMpIkYqwQHKEucNeLaGWqwpukIHvyJTNCFevzChTVrVnHEZXosNNnSDZIUUWwXOwhXtAXGsNAWQhtgz
BWPJRzUnqhyQqmHKMaMAbStxUkBYLCZjHjIfydlXEhPUBmbvGWMfDdgHTbHPhcHigNHPYFcKxFMbwScE
dcMflCWjIaGyqRYTTpBdHnxALgQqhvMHROKnIkXnPjcbKnUMZpmwkYnHquNPYkjfRWpXdcZEAtSYBJFa
xffrKRNZvpnbLUStqWNMXXVBkaHgMVUwMKrewUHhErylQKNFDFISZOQIqKNWJkawNfZfsycdyJvYMvlN
hWGixooELGuipQBdjVReiliXyYwgXfhgvlsBVNVBSjMDdTmiGxTDnancDvnLYooYItAdWxFiaaDIZxSt
kCMgActfmYykOCFoOeyRCDxHTycwgoSjzdvmCskFlQOajdQORhFWSbwfbAWDARPmCwvQPNdWFoVtHtXy
DBcDXJPvLCIfmfJyegfbFYUhtFlOvgZrAIbuqYkENDCWtRLqYbNzFCadQefKSwXcxoTJMgyljBxuQcSA
UAAMhperKxyrybCDNoxJxsEaGOycswcVTpYjCqZWWJwVzPDTdJJhmMaRBPYvCYhTQfBfeqFavffLblyR
xcSfBecyDrjjLBJLyFSCYAXLhXSfA                       wMFIGYLqioFOHDweFhrUAdxuKEcu
jZnvHJHkzBUzndPDWtSZxmZSNqWvr  flag-IGxKMshp46TgD3  IGrIHgZukyvqKilcRsvtianvNArf
lztuHCJgjNEIFGWkjHBKAzDFvNhQQ                       YrFbFRlCZbytBQnyjcUrkSpZkUQD
qIUBDxdSnAcLyrIcNdRFnQmnwVFejnnSjppghWruhrSluybnaMeHVZzxaXjJmeSbdTbzBTfstaVnIsyB
QuOPfyhpuqAYjdAmnrjEaHoNlgSprWnonSagzlHNyutcYLDqePPRaHGZBMkHwGSUNqMRtrdVmQsUZemR
cQwSrlctDTqreptPTpajphJwAjpbYBtrfJuxOVKytnSatMIuScasUkoHYiwISAjwDincTeHSuSymcyJI
CIlgBcVVFGMmhvydMHArlbhBLzeTauDtMBhlpxmygNpbwKMgFVJIRQBeKjKFCkOCBFcNUbWgEwdmdfFc
rocZhopuDATHyTFAXhuuAsaHzojWFoFRtFSqBNwPcYioTyKxKFiuHcyAZgznoynukJWQCujdvmXECoZY
EiEeDIMUVblOvGiUuBKPiYWpqPdFOrVLVZuJTikJfdNpVcXjhNgccKYlKuhaGebzMocmVEcRSaUMnoTY
lWaLiEyDTbDAPJKfnBrFdyHACbYYLZZkowukFMzRPERlMeDXnZwfaRtfbfrtEBhwYgsMhWRrsyKTXcjU
mZSUPuqweDeuzwFFWdkqnPUZGPqxJJueWxKhPYVlBjLYiEEsZiXFBCMBlmjRPvNUqbOVKSJdLqvXjNMb
tZONqajTHnFxdboJWgKwocLaOCOWVPvxnnJCuqhmQLuzHrQrHNdLcpIDzMmlJQsRiQsJbsNZXFnOWhYu
OIGvxjBFyRBABqXeuppGaXDVkKqsmzCxdCoCjHtadYuJDpghOdJahCcyrHMLgWteMzGBnNDAsbVWbpOr
```



