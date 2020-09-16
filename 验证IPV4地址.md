# 验证IPV4地址

编写一个函数来验证输入的字符串是否是有效的 IPv4 地址。
### 完整代码
```cpp
#include<stdio.h>
#include<iostream>
#include<string>
#include<vector>

using namespace std;

//如果小于0大于9则返回true，则返回false
bool largeThan9(string input) {
	for (int i = 0; i < input.size(); i++) {
		if (!(input[i] - '0' >= 0 && input[i] - '0' <= 9))
			return true;
	}
	return false;
}


//如果011则返回true，0则返回false
bool FirstIsZero(string input) {
	if (input.size() > 1 && input[0] == '0')
		return true;
	return false;
}

bool ValiIpv4(string ipv4) {
	string onebite = "";
	int start = 0;//记录“.”的个数
	vector<string> ipv4Vector;
	for (int i = 0; i < ipv4.size(); i++) {
		if (ipv4[i] == '.') {
			ipv4Vector.push_back(onebite);
			onebite = "";
			start++;
		}
		else {
			onebite.push_back(ipv4[i]);
		}
	}
	if (onebite.size() != 0)
		ipv4Vector.push_back(onebite);
	if (start != 3 || ipv4Vector.size() != 4)
		return false;
	for(int i = 0; i < ipv4Vector.size(); i++) {
		if (ipv4Vector[i].size() == 0 || ipv4Vector[i].size() > 32 || largeThan9(ipv4Vector[i]) || stoi(ipv4Vector[i]) > 255 || FirstIsZero(ipv4Vector[i]) ) {
			return false;
		}
	}
	return true;
}

int main() {
	string ipv4;
	cin >> ipv4;
	if (ValiIpv4(ipv4)) cout << "该地址为ipv4" << endl;
	else cout << "该地址不为ipv4" << endl;
	return 0;

}
```


