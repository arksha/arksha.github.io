---
layout: post
title: Word Brother's
date: 2016-01-21
categories: Leetcode
---


#Word Ladder#
[Word Ladder](https://leetcode.com/problems/word-ladder/)
 Solution: BFS, use hashmap to reduce repeat elements,and use queue to implement BFS

code:
 	
 	public class Solution {
    public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
        if(wordList.size()==0||wordList==null) return 0;
        HashSet<String> hash = new HashSet<>();
        Queue<String> q = new LinkedList<>();
        q.offer(beginWord);
        hash.add(beginWord);
        int length = 1;
        while(!q.isEmpty()){
            length++;
            int size = q.size();
            for(int i = 0;i<size;i++){
                String word = q.poll();
                for(String next: getnext(word,wordList)){
                    if(hash.contains(next))
                        continue;
                    if(next.equals(endWord))
                        return length;
                    hash.add(next);
                    q.offer(next);
                }
            }
        }
        return 0;
    }
    
    private String replace(String s, int index, char c) {
        char[] chars = s.toCharArray();
        chars[index] = c;
        return new String(chars);
    }
    
    private ArrayList<String> getnext(String word,Set<String> wordList){
        ArrayList<String> nextWords = new ArrayList<String>();
        for (char c = 'a'; c <= 'z'; c++) {
            for (int i = 0; i < word.length(); i++) {
                if (c == word.charAt(i)) {
                    continue;
                }
                String nextWord = replace(word, i, c);
                if (wordList.contains(nextWord)) {
                    nextWords.add(nextWord);
                }
            }
        }
        return nextWords;
    }
}


#Word Ladder ii#
[传送门](https://leetcode.com/problems/word-ladder-ii/)

solution：

code：

	public class Solution {
    public List<List<String>> findLadders(String start, String end,
            Set<String> dict) {
        List<List<String>> ladders = new ArrayList<List<String>>();
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        Map<String, Integer> distance = new HashMap<String, Integer>();

        dict.add(start);
        dict.add(end);
 
        bfs(map, distance, start, end, dict);
        
        List<String> path = new ArrayList<String>();
        
        dfs(ladders, path, end, start, distance, map);

        return ladders;
    }

    void dfs(List<List<String>> ladders, List<String> path, String crt,
            String start, Map<String, Integer> distance,
            Map<String, List<String>> map) {
        path.add(crt);
        if (crt.equals(start)) {
            Collections.reverse(path);
            ladders.add(new ArrayList<String>(path));
            Collections.reverse(path);
        } else {
            for (String next : map.get(crt)) {
                if (distance.containsKey(next) && distance.get(crt) == distance.get(next) + 1) { 
                    dfs(ladders, path, next, start, distance, map);
                }
            }           
        }
        path.remove(path.size() - 1);
    }

    void bfs(Map<String, List<String>> map, Map<String, Integer> distance,
            String start, String end, Set<String> dict) {
        Queue<String> q = new LinkedList<String>();
        q.offer(start);
        distance.put(start, 0);
        for (String s : dict) {
            map.put(s, new ArrayList<String>());
        }
        
        while (!q.isEmpty()) {
            String crt = q.poll();

            List<String> nextList = expand(crt, dict);
            for (String next : nextList) {
                map.get(next).add(crt);
                if (!distance.containsKey(next)) {
                    distance.put(next, distance.get(crt) + 1);
                    q.offer(next);
                }
            }
        }
    }

    List<String> expand(String crt, Set<String> dict) {
        List<String> expansion = new ArrayList<String>();

        for (int i = 0; i < crt.length(); i++) {
            for (char ch = 'a'; ch <= 'z'; ch++) {
                if (ch != crt.charAt(i)) {
                    String expanded = crt.substring(0, i) + ch
                            + crt.substring(i + 1);
                    if (dict.contains(expanded)) {
                        expansion.add(expanded);
                    }
                }
            }
        }

        return expansion;
    }
}

#Word Break#

solution:

code:

1.naive approach:

	public class Solution {
    public boolean wordBreak(String s, Set<String> dict) {
             return wordBreakHelper(s, dict, 0);
    }
 
	    public boolean wordBreakHelper(String s, Set<String> dict, int start){
	        if(start == s.length()) 
	            return true;
	 
	        for(String a: dict){
	            int len = a.length();
	            int end = start+len;
	 
	            //end index should be <= string length
	            if(end > s.length()) 
	                continue;
	 
	            if(s.substring(start, start+len).equals(a))
	                if(wordBreakHelper(s, dict, start+len))
	                    return true;
	        }
	 
	        return false;
	    }
	}

2.DP

code:

	public class Solution {
	    public boolean wordBreak(String s, Set<String> dict) {
	        boolean[] t = new boolean[s.length()+1];
	        t[0] = true; //set first to be true, why?
	        //Because we need initial state
	 
	        for(int i=0; i<s.length(); i++){
	            //should continue from match position
	            if(!t[i]) 
	                continue;
	 
	            for(String a: dict){
	                int len = a.length();
	                int end = i + len;
	                if(end > s.length())
	                    continue;
	 
	                if(t[end]) continue;
	 
	                if(s.substring(i, end).equals(a)){
	                    t[end] = true;
	                }
	            }
	        }
	 
	        return t[s.length()];
	    }
	}

#Word Break II#

solution: DP

code:

	public static List<String> wordBreak(String s, Set<String> dict) {
	    //create an array of ArrayList<String>
	    List<String> dp[] = new ArrayList[s.length()+1];
	    dp[0] = new ArrayList<String>();
	 
	    for(int i=0; i<s.length(); i++){
	        if( dp[i] == null ) 
	            continue; 
	 
	        for(String word:dict){
	            int len = word.length();
	            int end = i+len;
	            if(end > s.length()) 
	                continue;
	 
	            if(s.substring(i,end).equals(word)){
	                if(dp[end] == null){
	                    dp[end] = new ArrayList<String>();
	                }
	                dp[end].add(word);
	            }
	        }
	    }
	 
	    List<String> result = new LinkedList<String>();
	    if(dp[s.length()] == null) 
	        return result; 
	 
	    ArrayList<String> temp = new ArrayList<String>();
	    dfs(dp, s.length(), result, temp);
	 
	    return result;
	}
	 
	public static void dfs(List<String> dp[],int end,List<String> result, ArrayList<String> tmp){
	    if(end <= 0){
	        String path = tmp.get(tmp.size()-1);
	        for(int i=tmp.size()-2; i>=0; i--){
	            path += " " + tmp.get(i) ;
	        }
	 
	        result.add(path);
	        return;
	    }
	 
	    for(String str : dp[end]){
	        tmp.add(str);
	        dfs(dp, end-str.length(), result, tmp);
	        tmp.remove(tmp.size()-1);
	    }
	}

#Word Search#

solution:

code:

	public class Solution {
	    public boolean exist(char[][] board, String word) {
	        int n = board.length;
	        int m = board[0].length;
	        boolean res = false;
	        for(int i = 0;i<n;i++){
	            for(int j = 0;j<m;j++){
	                if(gothrough(i,j,board,word,0))
	                    res = true;
	            }
	        }

	      
	        return res;
	    }
	    public boolean gothrough(int a,int b, char[][] board,String word,int cur){
	        int num = word.length();
	        int n = board.length;
	        int m = board[0].length;
	            if(a>=n||a<0||b<0||b>=m)
	                return false;
	           
	            if(board[a][b]==word.charAt(cur)){
	                char temp = board[a][b];
	                board[a][b] = '#';
	                if(cur==num-1)
	                    return true;
	                else if(gothrough(a+1,b,board,word,cur+1)||
	                    gothrough(a,b+1,board,word,cur+1)||
	                    gothrough(a-1,b,board,word,cur+1)||
	                    gothrough(a,b-1,board,word,cur+1)
	                   )
	                    return true;  
	                
	              board[a][b] = temp;
	            }
	            return false;
    	}
	}

#Word Search II#

solution: use trie tree

code:

	public class Solution {
	    Trie trie = new Trie();
	    Set<String> res = new HashSet<String>(); 
	    public List<String> findWords(char[][] board, String[] words) {
	        
	         int m=board.length;
	         int n=board[0].length;
	        boolean mark[][] = new boolean[m][n];
	        for (int i = 0;i<words.length;i++)
	        {
	            trie.insert(words[i]);
	        }
	        for (int i = 0;i<board.length;i++)
	        {
	            for (int j = 0;j<board[0].length;j++)
	            {
	                dfs(board,"",i,j,trie,mark);
	            }
	        }
	     return new ArrayList<String>(res);   
	    }
	    

	    public void dfs(char[][] board,String str, int i,int j,Trie trie,boolean mark[][]){
	        int m=board.length;
	        int n=board[0].length;
	        
	        if(i<0||i>=m||j<0||j>=n)
	            return;
	        if(mark[i][j])
	            return;
	        str += board[i][j];
	        if(!trie.startsWith(str))
	            return;
	        if(trie.search(str))
	            if(!res.contains(str))
	            res.add(str);
	        mark[i][j] = true;
	        dfs(board,str,i-1,j,trie,mark);
	        dfs(board,str,i+1,j,trie,mark);
	        dfs(board,str,i,j+1,trie,mark);
	        dfs(board,str,i,j-1,trie,mark);
	        mark[i][j] = false;
	        
	    }
	}
	class TrieNode{
	    public TrieNode[] children = new TrieNode[26];
	    public String item = "";
	}
	 
	//Trie
	class Trie{
	    public TrieNode root = new TrieNode();
	 
	    public void insert(String word){
	        TrieNode node = root;
	        for(char c: word.toCharArray()){
	            if(node.children[c-'a']==null){
	                node.children[c-'a']= new TrieNode();
	            }
	            node = node.children[c-'a'];
	        }
	        node.item = word;
	    }
	 
	    public boolean search(String word){
	        TrieNode node = root;
	        for(char c: word.toCharArray()){
	            if(node.children[c-'a']==null)
	                return false;
	            node = node.children[c-'a'];
	        }
	        if(node.item.equals(word)){
	            return true;
	        }else{
	            return false;
	        }
	    }
	 
	    public boolean startsWith(String prefix){
	        TrieNode node = root;
	        for(char c: prefix.toCharArray()){
	            if(node.children[c-'a']==null)
	                return false;
	            node = node.children[c-'a'];
	        }
	        return true;
    	}
	}

#Word Pattern#

solution:

code:

	public class Solution {
	    public boolean wordPattern(String pattern, String str) {
	        String[] dict = str.split(" ");
	        if(pattern.length()!=dict.length){
	            return false;
	        }
	        Map index = new HashMap();
	        for(int i = 0;i<dict.length;i++){
	            if(!Objects.equals(index.put(pattern.charAt(i),i),
	                              index.put(dict[i],i)))
	                return false;
	           
	        }
	            return true;
	    }
	}