#include <iostream>
#include <vector>
using namespace std;

// Function to compute the Z-array
vector<int> computeZ(string s) {
    int n = s.length();
    vector<int> Z(n, 0);
    int L = 0, R = 0;

    for (int i = 1; i < n; i++) {
        if (i <= R) 
            Z[i] = min(R - i + 1, Z[i - L]);

        while (i + Z[i] < n && s[Z[i]] == s[i + Z[i]]) 
            Z[i]++;

        if (i + Z[i] - 1 > R) {
            L = i;
            R = i + Z[i] - 1;
        }
    }
    return Z;
}

// Function to find pattern occurrences using the Z-algorithm
void searchPattern(string pattern, string text) {
    string concat = pattern + "$" + text;
    vector<int> Z = computeZ(concat);

    int patternLength = pattern.length();
    for (int i = patternLength + 1; i < Z.size(); i++) {
        if (Z[i] == patternLength) 
            cout << "Pattern found at index " << i - patternLength - 1 << endl;
    }
}

// Main function
int main() {
    string text = "ababcababcab";
    string pattern = "abc";

    cout << "Text: " << text << endl;
    cout << "Pattern: " << pattern << endl;
    searchPattern(pattern, text);

    return 0;
}
