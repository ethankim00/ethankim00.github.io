---
title: "Data Structures and Algorithms for A Scrabble Like Word Game"
date: 2020-07-18T15:34:30-04:00
categories:
  - 
tags:
  
---



 During quarantine my family spent a lot of time playing bananagrams. Being our competitive selves we played a more fast-paced and intensive variation of the game known as Bandit. In Bandit players take turns flipping letter tiles. Whenever a word can be formed, the first player to spot it takes those letters and places the word in front of them. The twist is that letters can be added to an existing word to create a new word and thus players can steal words from each other. For example, Player A might have the word ‘car’ stolen by Player B who adds the letter e to spell race. A more complex example is adding the letter ‘t” 

Algorithm 
I wanted to build a tool to help me win the game and find new possible words to play. 
Given a word, all valid words that can be formed with one additional letter can be displayed. However, simple brute force search over all possibilities is highly inefficient, even at shorter lengths. Checking all possible permutations of a word with all possible additional letters is an O(n!) algorithm. By preprocessing the words, search time can be reduced to O(1). 

## Methods
To generate a dictionary of possible plays, I used google’s 30000 most common English words  as well as additional words that commonly arose during gameplay.  The word_forms package was used to generated all valid tenses and plurals of verbs that could be validly played. Overall there were ___ words.  Each word was mapped to it’s letters sorted in alphabetical order. For example, ‘read’ and ‘dare’ would map to ‘ader’. All words that are permutations of each other map to the same value. Now, the procedure to search for playable words given a word is as follows:

For each letter in the alphabet:

1. Append letter to word
2. Sort letters to get key
3. Look up words that map to that key


Thus possible words can be retrieved very quickly. 



[contact me](mailto:ethan_kim@college.harvard.edu)



