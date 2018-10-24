# Problem
Can you crack the key to [decrypt](https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/decrypt.py) [map2](https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/map2.txt) for us? The key to [map1](https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/map1.txt) is 11443513758266689915.

## Hints:
Have you heard of z3?

## Solution:

Lets download the files, and take a look:
```bash
wget https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/decrypt.py
wget https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/map1.txt
wget https://2018shell1.picoctf.com/static/e41fb3c1099a2f0f52f85de0299aadb2/map2.txt
```

decrypt.py:
```python
#!/usr/bin/python2

from hashlib import sha512
import sys

def verify(x, chalbox):
    length, gates, check = chalbox
    b = [(x >> i) & 1 for i in range(length)]
    for name, args in gates:
        if name == 'true':
            b.append(1)
        else:
            u1 = b[args[0][0]] ^ args[0][1]
            u2 = b[args[1][0]] ^ args[1][1]
            if name == 'or':
                b.append(u1 | u2)
            elif name == 'xor':
                b.append(u1 ^ u2)
    return b[check[0]] ^ check[1]
    
def dec(x, w):
    z = int(sha512(str(int(x))).hexdigest(), 16)
    return '{:x}'.format(w ^ z).decode('hex')

if __name__ == '__main__':
    if len(sys.argv) < 3:
        print 'Usage: ' + sys.argv[0] + ' <key> <map.txt>'
        print 'Example: Try Running ' + sys.argv[0] + ' 11443513758266689915 map1.txt'
        exit(1)
    with open(sys.argv[2], 'r') as f:
        cipher, chalbox = eval(f.read())
    
    key = int(sys.argv[1]) % (1 << chalbox[0])
    print 'Attempting to decrypt ' + sys.argv[2] + '...'
    if verify(key, chalbox):
        print 'Congrats the flag for ' + sys.argv[2] + ' is:', dec(key, cipher)
    else:
        print 'Wrong key for ' + sys.argv[2] + '.'
```

This code verifing the key (check if the key satisfies the circuit), and then decrypting the cipher with it.

map1.txt:
```
(1091562780682878452932647567206562803258945860781462102555439111325671293639822353361220777655154004326830877696329866178864341430343894025596404608627826L, (64, [('true', []), ('xor', [(0, False), (64, False)]), ('xor', [(65, False), (64, True)]), ('or', [(64, True), (64, False)]), ('or', [(0, True), (64, False)]), ('or', [(67, True), (68, True)]), ('or', [(0, True), (64, True)]), ('or', [(69, False), (70, True)]), ('xor', [(1, False), (64, False)]), ('xor', [(72, False), (71, False)]), ('or', [(64, True), (71, True)]), ('or', [(1, True), (71, True)]), ('or', [(74, True), (75, True)]), ('or', [(1, True), (64, True)]), ('or', [(76, False), (77, True)]), ('xor', [(2, False), (64, False)]), ('xor', [(79, False), (78, False)]), ('or', [(64, True), (78, True)]), ('or', [(2, True), (78, True)]), ('or', [(81, True), (82, True)]), ('or', [(2, True), (64, True)]), ('or', [(83, False), (84, True)]), ('xor', [(3, False), (64, True)]), ('xor', [(86, False), (85, False)]), ('or', [(64, False), (85, True)]), ('or', [(3, True), (85, True)]), ('or', [(88, True), (89, True)]), ('or', [(3, True), (64, False)]), ('or', [(90, False), (91, True)]), ('xor', [(4, False), (64, False)]), ('xor', [(93, False), (92, False)]), ('or', [(64, True), (92, True)]), ('or', [(4, True), (92, True)]), ('or', [(95, True), (96, True)]), ('or', [(4, True), (64, True)]), ('or', [(97, False), (98, True)]), ('xor', [(5, False), (64, True)]), ('xor', [(100, False), (99, False)]), ('or', [(64, False), (99, True)]), ('or', [(5, True), (99, True)]), ('or', [(102, True), (103, True)]), ('or', [(5, True), (64, False)]), ('or', [(104, False), (105, True)]), ('xor', [(6, False), (64, False)]), ('xor', [(107, False), (106, False)]), ('or', [(64, True), (106, True)]), ('or', [(6, True), (106, True)]), ('or', [(109, True), (110, True)]), ('or', [(6, True), (64, True)]), ('or', [(111, False), (112, True)]), ('xor', [(7, False), (64, False)]), ('xor', [(114, False), (113, False)]), ('or', [(64, True), (113, True)]), ('or', [(7, True), (113, True)]), ('or', [(116, True), (117, True)]), ('or', [(7, True), (64, True)]), ('or', [(118, False), (119, True)]), ('xor', [(8, False), (64, False)]), ('xor', [(121, False), (120, False)]), ('or', [(64, True), (120, True)]), ('or', [(8, True), (120, True)]), ('or', [(123, True), (124, True)]), ('or', [(8, True), (64, True)]), ('or', [(125, False), (126, True)]), ('xor', [(9, False), (64, False)]), ('xor', [(128, False), (127, False)]), ('or', [(64, True), (127, True)]), ('or', [(9, True), (127, True)]), ('or', [(130, True), (131, True)]), ('or', [(9, True), (64, True)]), ('or', [(132, False), (133, True)]), ('xor', [(10, False), (64, False)]), ('xor', [(135, False), (134, False)]), ('or', [(64, True), (134, True)]), ('or', [(10, True), (134, True)]), ('or', [(137, True), (138, True)]), ('or', [(10, True), (64, True)]), ('or', [(139, False), (140, True)]), ('xor', [(11, False), (64, True)]), ('xor', [(142, False), (141, False)]), ('or', [(64, False), (141, True)]), ('or', [(11, True), (141, True)]), ('or', [(144, True), (145, True)]), ('or', [(11, True), (64, False)]), ('or', [(146, False), (147, True)]), ('xor', [(12, False), (64, True)]), ('xor', [(149, False), (148, False)]), ('or', [(64, False), (148, True)]), ('or', [(12, True), (148, True)]), ('or', [(151, True), (152, True)]), ('or', [(12, True), (64, False)]), ('or', [(153, False), (154, True)]), ('xor', [(13, False), (64, False)]), ('xor', [(156, False), (155, False)]), ('or', [(64, True), (155, True)]), ('or', [(13, True), (155, True)]), ('or', [(158, True), (159, True)]), ('or', [(13, True), (64, True)]), ('or', [(160, False), (161, True)]), ('xor', [(14, False), (64, True)]), ('xor', [(163, False), (162, False)]), ('or', [(64, False), (162, True)]), ('or', [(14, True), (162, True)]), ('or', [(165, True), (166, True)]), ('or', [(14, True), (64, False)]), ('or', [(167, False), (168, True)]), ('xor', [(15, False), (64, True)]), ('xor', [(170, False), (169, False)]), ('or', [(64, False), (169, True)]), ('or', [(15, True), (169, True)]), ('or', [(172, True), (173, True)]), ('or', [(15, True), (64, False)]), ('or', [(174, False), (175, True)]), ('xor', [(16, False), (64, True)]), ('xor', [(177, False), (176, False)]), ('or', [(64, False), (176, True)]), ('or', [(16, True), (176, True)]), ('or', [(179, True), (180, True)]), ('or', [(16, True), (64, False)]), ('or', [(181, False), (182, True)]), ('xor', [(17, False), (64, True)]), ('xor', [(184, False), (183, False)]), ('or', [(64, False), (183, True)]), ('or', [(17, True), (183, True)]), ('or', [(186, True), (187, True)]), ('or', [(17, True), (64, False)]), ('or', [(188, False), (189, True)]), ('xor', [(18, False), (64, True)]), ('xor', [(191, False), (190, False)]), ('or', [(64, False), (190, True)]), ('or', [(18, True), (190, True)]), ('or', [(193, True), (194, True)]), ('or', [(18, True), (64, False)]), ('or', [(195, False), (196, True)]), ('xor', [(19, False), (64, False)]), ('xor', [(198, False), (197, False)]), ('or', [(64, True), (197, True)]), ('or', [(19, True), (197, True)]), ('or', [(200, True), (201, True)]), ('or', [(19, True), (64, True)]), ('or', [(202, False), (203, True)]), ('xor', [(20, False), (64, True)]), ('xor', [(205, False), (204, False)]), ('or', [(64, False), (204, True)]), ('or', [(20, True), (204, True)]), ('or', [(207, True), (208, True)]), ('or', [(20, True), (64, False)]), ('or', [(209, False), (210, True)]), ('xor', [(21, False), (64, False)]), ('xor', [(212, False), (211, False)]), ('or', [(64, True), (211, True)]), ('or', [(21, True), (211, True)]), ('or', [(214, True), (215, True)]), ('or', [(21, True), (64, True)]), ('or', [(216, False), (217, True)]), ('xor', [(22, False), (64, True)]), ('xor', [(219, False), (218, False)]), ('or', [(64, False), (218, True)]), ('or', [(22, True), (218, True)]), ('or', [(221, True), (222, True)]), ('or', [(22, True), (64, False)]), ('or', [(223, False), (224, True)]), ('xor', [(23, False), (64, False)]), ('xor', [(226, False), (225, False)]), ('or', [(64, True), (225, True)]), ('or', [(23, True), (225, True)]), ('or', [(228, True), (229, True)]), ('or', [(23, True), (64, True)]), ('or', [(230, False), (231, True)]), ('xor', [(24, False), (64, True)]), ('xor', [(233, False), (232, False)]), ('or', [(64, False), (232, True)]), ('or', [(24, True), (232, True)]), ('or', [(235, True), (236, True)]), ('or', [(24, True), (64, False)]), ('or', [(237, False), (238, True)]), ('xor', [(25, False), (64, False)]), ('xor', [(240, False), (239, False)]), ('or', [(64, True), (239, True)]), ('or', [(25, True), (239, True)]), ('or', [(242, True), (243, True)]), ('or', [(25, True), (64, True)]), ('or', [(244, False), (245, True)]), ('xor', [(26, False), (64, True)]), ('xor', [(247, False), (246, False)]), ('or', [(64, False), (246, True)]), ('or', [(26, True), (246, True)]), ('or', [(249, True), (250, True)]), ('or', [(26, True), (64, False)]), ('or', [(251, False), (252, True)]), ('xor', [(27, False), (64, True)]), ('xor', [(254, False), (253, False)]), ('or', [(64, False), (253, True)]), ('or', [(27, True), (253, True)]), ('or', [(256, True), (257, True)]), ('or', [(27, True), (64, False)]), ('or', [(258, False), (259, True)]), ('xor', [(28, False), (64, False)]), ('xor', [(261, False), (260, False)]), ('or', [(64, True), (260, True)]), ('or', [(28, True), (260, True)]), ('or', [(263, True), (264, True)]), ('or', [(28, True), (64, True)]), ('or', [(265, False), (266, True)]), ('xor', [(29, False), (64, True)]), ('xor', [(268, False), (267, False)]), ('or', [(64, False), (267, True)]), ('or', [(29, True), (267, True)]), ('or', [(270, True), (271, True)]), ('or', [(29, True), (64, False)]), ('or', [(272, False), (273, True)]), ('xor', [(30, False), (64, False)]), ('xor', [(275, False), (274, False)]), ('or', [(64, True), (274, True)]), ('or', [(30, True), (274, True)]), ('or', [(277, True), (278, True)]), ('or', [(30, True), (64, True)]), ('or', [(279, False), (280, True)]), ('xor', [(31, False), (64, True)]), ('xor', [(282, False), (281, False)]), ('or', [(64, False), (281, True)]), ('or', [(31, True), (281, True)]), ('or', [(284, True), (285, True)]), ('or', [(31, True), (64, False)]), ('or', [(286, False), (287, True)]), ('xor', [(32, False), (64, True)]), ('xor', [(289, False), (288, False)]), ('or', [(64, False), (288, True)]), ('or', [(32, True), (288, True)]), ('or', [(291, True), (292, True)]), ('or', [(32, True), (64, False)]), ('or', [(293, False), (294, True)]), ('xor', [(33, False), (64, False)]), ('xor', [(296, False), (295, False)]), ('or', [(64, True), (295, True)]), ('or', [(33, True), (295, True)]), ('or', [(298, True), (299, True)]), ('or', [(33, True), (64, True)]), ('or', [(300, False), (301, True)]), ('xor', [(34, False), (64, False)]), ('xor', [(303, False), (302, False)]), ('or', [(64, True), (302, True)]), ('or', [(34, True), (302, True)]), ('or', [(305, True), (306, True)]), ('or', [(34, True), (64, True)]), ('or', [(307, False), (308, True)]), ('xor', [(35, False), (64, False)]), ('xor', [(310, False), (309, False)]), ('or', [(64, True), (309, True)]), ('or', [(35, True), (309, True)]), ('or', [(312, True), (313, True)]), ('or', [(35, True), (64, True)]), ('or', [(314, False), (315, True)]), ('xor', [(36, False), (64, False)]), ('xor', [(317, False), (316, False)]), ('or', [(64, True), (316, True)]), ('or', [(36, True), (316, True)]), ('or', [(319, True), (320, True)]), ('or', [(36, True), (64, True)]), ('or', [(321, False), (322, True)]), ('xor', [(37, False), (64, True)]), ('xor', [(324, False), (323, False)]), ('or', [(64, False), (323, True)]), ('or', [(37, True), (323, True)]), ('or', [(326, True), (327, True)]), ('or', [(37, True), (64, False)]), ('or', [(328, False), (329, True)]), ('xor', [(38, False), (64, True)]), ('xor', [(331, False), (330, False)]), ('or', [(64, False), (330, True)]), ('or', [(38, True), (330, True)]), ('or', [(333, True), (334, True)]), ('or', [(38, True), (64, False)]), ('or', [(335, False), (336, True)]), ('xor', [(39, False), (64, False)]), ('xor', [(338, False), (337, False)]), ('or', [(64, True), (337, True)]), ('or', [(39, True), (337, True)]), ('or', [(340, True), (341, True)]), ('or', [(39, True), (64, True)]), ('or', [(342, False), (343, True)]), ('xor', [(40, False), (64, True)]), ('xor', [(345, False), (344, False)]), ('or', [(64, False), (344, True)]), ('or', [(40, True), (344, True)]), ('or', [(347, True), (348, True)]), ('or', [(40, True), (64, False)]), ('or', [(349, False), (350, True)]), ('xor', [(41, False), (64, True)]), ('xor', [(352, False), (351, False)]), ('or', [(64, False), (351, True)]), ('or', [(41, True), (351, True)]), ('or', [(354, True), (355, True)]), ('or', [(41, True), (64, False)]), ('or', [(356, False), (357, True)]), ('xor', [(42, False), (64, False)]), ('xor', [(359, False), (358, False)]), ('or', [(64, True), (358, True)]), ('or', [(42, True), (358, True)]), ('or', [(361, True), (362, True)]), ('or', [(42, True), (64, True)]), ('or', [(363, False), (364, True)]), ('xor', [(43, False), (64, True)]), ('xor', [(366, False), (365, False)]), ('or', [(64, False), (365, True)]), ('or', [(43, True), (365, True)]), ('or', [(368, True), (369, True)]), ('or', [(43, True), (64, False)]), ('or', [(370, False), (371, True)]), ('xor', [(44, False), (64, True)]), ('xor', [(373, False), (372, False)]), ('or', [(64, False), (372, True)]), ('or', [(44, True), (372, True)]), ('or', [(375, True), (376, True)]), ('or', [(44, True), (64, False)]), ('or', [(377, False), (378, True)]), ('xor', [(45, False), (64, False)]), ('xor', [(380, False), (379, False)]), ('or', [(64, True), (379, True)]), ('or', [(45, True), (379, True)]), ('or', [(382, True), (383, True)]), ('or', [(45, True), (64, True)]), ('or', [(384, False), (385, True)]), ('xor', [(46, False), (64, False)]), ('xor', [(387, False), (386, False)]), ('or', [(64, True), (386, True)]), ('or', [(46, True), (386, True)]), ('or', [(389, True), (390, True)]), ('or', [(46, True), (64, True)]), ('or', [(391, False), (392, True)]), ('xor', [(47, False), (64, True)]), ('xor', [(394, False), (393, False)]), ('or', [(64, False), (393, True)]), ('or', [(47, True), (393, True)]), ('or', [(396, True), (397, True)]), ('or', [(47, True), (64, False)]), ('or', [(398, False), (399, True)]), ('xor', [(48, False), (64, True)]), ('xor', [(401, False), (400, False)]), ('or', [(64, False), (400, True)]), ('or', [(48, True), (400, True)]), ('or', [(403, True), (404, True)]), ('or', [(48, True), (64, False)]), ('or', [(405, False), (406, True)]), ('xor', [(49, False), (64, True)]), ('xor', [(408, False), (407, False)]), ('or', [(64, False), (407, True)]), ('or', [(49, True), (407, True)]), ('or', [(410, True), (411, True)]), ('or', [(49, True), (64, False)]), ('or', [(412, False), (413, True)]), ('xor', [(50, False), (64, True)]), ('xor', [(415, False), (414, False)]), ('or', [(64, False), (414, True)]), ('or', [(50, True), (414, True)]), ('or', [(417, True), (418, True)]), ('or', [(50, True), (64, False)]), ('or', [(419, False), (420, True)]), ('xor', [(51, False), (64, False)]), ('xor', [(422, False), (421, False)]), ('or', [(64, True), (421, True)]), ('or', [(51, True), (421, True)]), ('or', [(424, True), (425, True)]), ('or', [(51, True), (64, True)]), ('or', [(426, False), (427, True)]), ('xor', [(52, False), (64, True)]), ('xor', [(429, False), (428, False)]), ('or', [(64, False), (428, True)]), ('or', [(52, True), (428, True)]), ('or', [(431, True), (432, True)]), ('or', [(52, True), (64, False)]), ('or', [(433, False), (434, True)]), ('xor', [(53, False), (64, False)]), ('xor', [(436, False), (435, False)]), ('or', [(64, True), (435, True)]), ('or', [(53, True), (435, True)]), ('or', [(438, True), (439, True)]), ('or', [(53, True), (64, True)]), ('or', [(440, False), (441, True)]), ('xor', [(54, False), (64, False)]), ('xor', [(443, False), (442, False)]), ('or', [(64, True), (442, True)]), ('or', [(54, True), (442, True)]), ('or', [(445, True), (446, True)]), ('or', [(54, True), (64, True)]), ('or', [(447, False), (448, True)]), ('xor', [(55, False), (64, True)]), ('xor', [(450, False), (449, False)]), ('or', [(64, False), (449, True)]), ('or', [(55, True), (449, True)]), ('or', [(452, True), (453, True)]), ('or', [(55, True), (64, False)]), ('or', [(454, False), (455, True)]), ('xor', [(56, False), (64, True)]), ('xor', [(457, False), (456, False)]), ('or', [(64, False), (456, True)]), ('or', [(56, True), (456, True)]), ('or', [(459, True), (460, True)]), ('or', [(56, True), (64, False)]), ('or', [(461, False), (462, True)]), ('xor', [(57, False), (64, False)]), ('xor', [(464, False), (463, False)]), ('or', [(64, True), (463, True)]), ('or', [(57, True), (463, True)]), ('or', [(466, True), (467, True)]), ('or', [(57, True), (64, True)]), ('or', [(468, False), (469, True)]), ('xor', [(58, False), (64, True)]), ('xor', [(471, False), (470, False)]), ('or', [(64, False), (470, True)]), ('or', [(58, True), (470, True)]), ('or', [(473, True), (474, True)]), ('or', [(58, True), (64, False)]), ('or', [(475, False), (476, True)]), ('xor', [(59, False), (64, False)]), ('xor', [(478, False), (477, False)]), ('or', [(64, True), (477, True)]), ('or', [(59, True), (477, True)]), ('or', [(480, True), (481, True)]), ('or', [(59, True), (64, True)]), ('or', [(482, False), (483, True)]), ('xor', [(60, False), (64, True)]), ('xor', [(485, False), (484, False)]), ('or', [(64, False), (484, True)]), ('or', [(60, True), (484, True)]), ('or', [(487, True), (488, True)]), ('or', [(60, True), (64, False)]), ('or', [(489, False), (490, True)]), ('xor', [(61, False), (64, True)]), ('xor', [(492, False), (491, False)]), ('or', [(64, False), (491, True)]), ('or', [(61, True), (491, True)]), ('or', [(494, True), (495, True)]), ('or', [(61, True), (64, False)]), ('or', [(496, False), (497, True)]), ('xor', [(62, False), (64, True)]), ('xor', [(499, False), (498, False)]), ('or', [(64, False), (498, True)]), ('or', [(62, True), (498, True)]), ('or', [(501, True), (502, True)]), ('or', [(62, True), (64, False)]), ('or', [(503, False), (504, True)]), ('xor', [(63, False), (64, False)]), ('xor', [(506, False), (505, False)]), ('or', [(66, False), (73, True)]), ('or', [(80, False), (87, False)]), ('or', [(508, False), (509, False)]), ('or', [(94, True), (101, False)]), ('or', [(108, True), (115, False)]), ('or', [(511, False), (512, False)]), ('or', [(510, False), (513, False)]), ('or', [(122, True), (129, False)]), ('or', [(136, False), (143, False)]), ('or', [(515, False), (516, False)]), ('or', [(150, True), (157, True)]), ('or', [(164, False), (171, False)]), ('or', [(518, False), (519, False)]), ('or', [(517, False), (520, False)]), ('or', [(514, False), (521, False)]), ('or', [(178, False), (185, False)]), ('or', [(192, False), (199, False)]), ('or', [(523, False), (524, False)]), ('or', [(206, True), (213, True)]), ('or', [(220, True), (227, False)]), ('or', [(526, False), (527, False)]), ('or', [(525, False), (528, False)]), ('or', [(234, False), (241, True)]), ('or', [(248, False), (255, False)]), ('or', [(530, False), (531, False)]), ('or', [(262, True), (269, False)]), ('or', [(276, True), (283, False)]), ('or', [(533, False), (534, False)]), ('or', [(532, False), (535, False)]), ('or', [(529, False), (536, False)]), ('or', [(522, False), (537, False)]), ('or', [(290, False), (297, False)]), ('or', [(304, False), (311, False)]), ('or', [(539, False), (540, False)]), ('or', [(318, False), (325, False)]), ('or', [(332, True), (339, True)]), ('or', [(542, False), (543, False)]), ('or', [(541, False), (544, False)]), ('or', [(346, True), (353, True)]), ('or', [(360, False), (367, True)]), ('or', [(546, False), (547, False)]), ('or', [(374, False), (381, True)]), ('or', [(388, True), (395, True)]), ('or', [(549, False), (550, False)]), ('or', [(548, False), (551, False)]), ('or', [(545, False), (552, False)]), ('or', [(402, True), (409, True)]), ('or', [(416, True), (423, False)]), ('or', [(554, False), (555, False)]), ('or', [(430, True), (437, True)]), ('or', [(444, False), (451, False)]), ('or', [(557, False), (558, False)]), ('or', [(556, False), (559, False)]), ('or', [(458, True), (465, False)]), ('or', [(472, False), (479, True)]), ('or', [(561, False), (562, False)]), ('or', [(486, False), (493, True)]), ('or', [(500, False), (507, False)]), ('or', [(564, False), (565, False)]), ('or', [(563, False), (566, False)]), ('or', [(560, False), (567, False)]), ('or', [(553, False), (568, False)]), ('or', [(538, False), (569, False)])], (570, True)))
```

This file contains the (demo) ciphertext and the (demo) circuit.

We can use [z3](https://github.com/Z3Prover/z3) to solve it easily!

Code (we just let ```verify``` to build the constraint for us and feed it to z3):
```python
#!/usr/bin/env python

from z3 import *
import sys

from decrypt import dec


def verify(x, chalbox):
	length, gates, check = chalbox

	b = [(x >> i) & 1 for i in range(length)]
	for name, args in gates:
		if name == 'true':
			b.append(1)
		else:
			u1 = b[args[0][0]] ^ args[0][1]
			u2 = b[args[1][0]] ^ args[1][1]
			if name == 'or':
				b.append(u1 | u2)
			elif name == 'xor':
				b.append(u1 ^ u2)

	s.add(b[check[0]] ^ check[1] == 1)

	return b[check[0]] ^ check[1]

if len(sys.argv) < 2:
	print 'Usage: ' + sys.argv[0] + ' <map.txt>'

	sys.exit(1)

with open(sys.argv[1], 'r') as f:
	cipher, chalbox = eval(f.read())

s = Solver()
key = BitVec('key', 128)
verify(key, chalbox)

s.check()
key = str(s.model()[key])

print dec(key, cipher)
```

Flag: picoCTF{36cc0cc10d273941c34694abdb21580d__aw350m3_ari7hm37ic__}