# 3. Extend TrieMatching

## Problem Description

**Task.** Extend `TrieMatching` algorithm so that it handles correctly cases when one of the patterns is a prefix of another one.

**Input Format.** The first line of the input contains a string _Text_, the second line contains an integer _n_, each of the following _n_ lines contains a pattern from Patterns = {_p_<sub>_1_</sub>, . . . , _p_<sub>_n_</sub>}.

**Constraints.** 1 ≤ _n_ ≤ 100; 1 ≤ |_p_<sub>_i_</sub>| ≤ 100 for all 1 ≤ _i_ ≤ _n_; _p_<sub>_i_</sub>’s contain only symbols A, C, G, T; no _p_<sub>_i_</sub> is a prefix of _p_<sub>_j_</sub> for all 1 ≤ _i_ ̸= _j_ ≤ _n_; it can be the case that _p_<sub>_i_</sub> is a prefx of _p_<sub>_j_</sub> for some _i_, _j_.

**Output Format.** All starting positions in _Text_ where a string from _Patterns_ appears as a substring in increasing order (assuming that _Text_ is a 0-based array of symbols). If more than one pattern appears starting at position _i_, output _i_ once.

**Sample 1.**

```
    Input
        AAA
        1
        AA
    Output
        0 1
```

**Sample 2.**

```
    Input
        ACATA
        3
        AT
        A
        AG
    Output
        0 2 4
```

## Solution

```java
List<Node> buildTrie(List<String> patterns) {
    List<Node> trie = new ArrayList<>();
    trie.add(new Node());

    for (String pattern : patterns) {
        int curr = 0;
        for (int i = 0; i < pattern.length(); i++) {
            char symbol = pattern.charAt(i);
            Node n = trie.get(curr);
            int idx = letterToIndex(symbol);

            if (n.next[idx] != Node.NA) {
                curr = n.next[idx];
            } else {
                trie.add(new Node());
                n.next[idx] = trie.size() - 1;
                curr = trie.size() - 1;
            }
        }
        trie.get(curr).patternEnd = true;
    }
    return trie;
}
```

```java
int prefixTrieMatching(String text, int start, List<Node> trie) {
    for (int i = start, v = 0; i < text.length(); i++) {
        char symbol = text.charAt(i);
        int idx = letterToIndex(symbol);
        v = trie.get(v).next[idx];

        if (v == Node.NA)
            return -1;
        else if (trie.get(v).patternEnd)
            return start;
    }

    return -1;
}
```

```java
List <Integer> solve (String text, int n, List <String> patterns) {
    List <Integer> result = new ArrayList <Integer> ();
    List<Node> trie = buildTrie(patterns);
    
    for (int i = 0; i < text.length(); i++) {
        int pos = prefixTrieMatching(text, i, trie);

        if (pos >= 0)
            result.add(pos);
    }

    return result;
}
```

## Test

Compile with `javac TrieMatchingExtended.java` and run with `java TrieMatchingExtended`.


**This is only for discussion and communication. Please don't use this for submission of assignments.**