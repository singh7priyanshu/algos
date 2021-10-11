{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Dynamic Programming\n",
    "\n",
    "- [0/1 Knapsack](#0/1-Knapsack)\n",
    " - Methods to solve 0/1 Knapsack\n",
    "   - [Recursive Method](#Recursive-Method)\n",
    "   - [Memomization Method](#Memomization-Method)\n",
    "   - [Top-Down Approach](#Top-Down-Approach)\n",
    " - Problems based on 0/1 Knapsack\n",
    "   - [Subset Sum Problem](#Subset-Sum-Problem)\n",
    "   - [Equal Sum Partition](#Equal-Sum-Partition)\n",
    "   - [Subset with sum divisible by m](#Subset-with-sum-divisible-by-m)\n",
    "   - [Count of Subsets of a given Sum](#Count-of-Subsets-of-a-given-Sum)\n",
    "   - [Minimum Subset Sum Difference](#Minimum-Subset-Sum-Difference)\n",
    "   - [Count the number of subset with a given difference](#Count-the-number-of-subset-with-a-given-difference)\n",
    "   - [Target Sum](#Target-Sum-(same-as-num_subset_given_diff))\n",
    "- [Unbounded Knapsack](#Unbounded-Knapsack)\n",
    " - Problems based on Unbounded Knapsack\n",
    "   - [Rod Cutting Problem](#Rod-Cutting-Problem)\n",
    "   - [Maximize the number of segments of length p, q and r](#Maximize-the-number-of-segments-of-length-p,-q-and-r)\n",
    "   - [Coin change problem : Maximum number of ways](#Coin-change-problem-:-Maximum-number-of-ways)\n",
    "   - [Coin change problem : Minimum number of coins](#Coin-change-problem-:-Minimum-number-of-coins)\n",
    "- [Longest common subsequence](#Longest-common-subsequence)\n",
    " - Methods to solve LCS Problems\n",
    "   - [Using Recursion](#Using-Recursion)\n",
    "   - [Memomization](#Memomization)\n",
    "   - [Top Down Approach](#Top-Down-Approach)\n",
    " - Problesm based on LCS\n",
    "   - [Longest Common Substring](#Longest-Common-Substring)\n",
    "   - [Print Longest Common subsequence](#Print-Longest-Common-subsequence)\n",
    "   - [Print Longest common substring](#Print-Longest-common-substring)\n",
    "   - [Shortest Common SuperSubsequence](#Shortest-Common-SuperSubsequence)\n",
    "   - [Minimum number of insertion and deletion to convert a string X to Y](#Minimum-number-of-insertion-and-deletion-to-convert-a-string-X-to-Y)\n",
    "   - [Longest Palindromic Subsequence](#Longest-Palindromic-Subsequence)\n",
    "   - [Minimum number of deletion to make a string palindrome](#Minimum-number-of-deletion-to-make-a-string-palindrome)\n",
    "   - [Print Shortest Common SuperSequence](#Print-Shortest-Common-SuperSequence)\n",
    "   - [Longest Repeating SubSequence](#Longest-Repeating-SubSequence)\n",
    "   - [Sequence Pattern Matching](#Sequence-Pattern-Matching)\n",
    "   - [Minimum number of insertion to make a string palindrome](#Minimum-number-of-insertion-to-make-a-string-palindrome)\n",
    "- [Matrix Chain Multiplication](#Matrix-Chain-Multiplication) \n",
    " - Methods to Solve Matrix Chain Multiplication\n",
    "   - [Recursive](#Recursive-Approach)\n",
    "   - [Memomization](#Matrix-Chain-Multiplication-Memomization)\n",
    " - Problems based on Matrix Chain Multiplication\n",
    "   - [Palindrome Partitioning](#Palindrome-Partitioning)\n",
    "   - [Evaluate expression to True (Boolean Parenthisization)](#Evaluate-expression-to-True-(Boolean-Parenthisization))\n",
    "   - [Scrambled String](#Scrambled-String)\n",
    "   - [Egg Droping Problem](#Egg-Droping-Problem)\n",
    "- [Others](#Others)\n",
    " - [Length of Longest Bitonic Subsequence](#Length-of-Longest-Bitonic-Subsequence)\n",
    "  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [0/1 Knapsack](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)\n",
    "\n",
    "Given weights and values of n items, put these items in a knapsack of capacity W to get the maximum total value in the knapsack. In other words, given two integer arrays val[0..n-1] and wt[0..n-1] which represent values and weights associated with n items respectively. Also given an integer W which represents knapsack capacity, find out the maximum value subset of val[] such that sum of the weights of this subset is smaller than or equal to W. You cannot break an item, either pick the complete item or don’t pick it (0-1 property).\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "wt = [1, 2, 3, 4, 5, 6]\n",
    "val = [6, 1, 9, 4, 2, 8]\n",
    "w = 7"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Recursive Method"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "75"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def recursive_01_knapsack(wt, val, w, n):\n",
    "    if n == 0 or w == 0:\n",
    "        return 0\n",
    "    \n",
    "    if wt[n-1] <= w:\n",
    "        taken = val[n-1] + recursive_01_knapsack(wt, val, w-wt[n-1], n-1)\n",
    "        not_taken = recursive_01_knapsack(wt, val, w, n-1)\n",
    "        return max(taken, not_taken)\n",
    "    else:\n",
    "        return recursive_01_knapsack(wt, val, w, n-1)\n",
    "\n",
    "\n",
    "recursive_01_knapsack(wt, val, w, len(wt))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Memomization Method"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "t = [[-1 for _ in range(w+1)] for _ in range(len(wt)+1)]\n",
    "def memomization_01_knapsack(wt, val, w, n):\n",
    "    if n == 0 or w == 0:\n",
    "        return 0\n",
    "    \n",
    "    if t[n][w] != -1:\n",
    "        return t[n][w]\n",
    "    elif wt[n-1] <= w:\n",
    "        taken = val[n-1] + memomization_01_knapsack(wt, val, w-wt[n-1], n-1)\n",
    "        not_taken = memomization_01_knapsack(wt, val, w, n-1)\n",
    "        t[n][w] = max(taken, not_taken)\n",
    "    else:\n",
    "        t[n][w] = memomization_01_knapsack(wt, val, w, n-1)\n",
    "    \n",
    "    return t[n][w]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "75"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "memomization_01_knapsack(wt, val, w, len(wt))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Top-Down Approach"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "75"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def top_down_01_knapsack(wt, val, w, n):\n",
    "    t = [[-1 for _ in range(w+1)] for _ in range(n+1)]\n",
    "    for i in range(n+1):\n",
    "        for j in range(w+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif wt[i-1] <= j:\n",
    "                t[i][j] = max(val[i-1] + t[i-1][j - wt[i-1]], t[i-1][j])\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return t[n][w]\n",
    "\n",
    "top_down_01_knapsack(wt, val, w, len(wt))"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Subset Sum Problem](https://www.geeksforgeeks.org/subset-sum-problem-dp-25/)\n",
    "\n",
    "Given a set of non-negative integers, and a value sum, determine if there is a subset of the given set with sum equal to given sum.\n",
    "\n",
    "#### Input / Output\n",
    "\n",
    "Input:arr = [3, 34, 4, 12, 5, 2], s = 9\n",
    "\n",
    "Output: True\n",
    "\n",
    "There is a subset (4, 5) with sum 9."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def subset_sum_problem(arr, s):\n",
    "    n = len(arr)\n",
    "    t = [[False for _ in range(s+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if i == 0:\n",
    "                t[i][j] = False\n",
    "                t[0][0] = False\n",
    "            if j == 0:\n",
    "                t[i][j] = True\n",
    "            elif arr[i-1] <= j:\n",
    "                t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    return t[i][j]\n",
    "\n",
    "subset_sum_problem([3, 34, 4, 12, 5, 2], 9)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Equal Sum Partition](https://www.geeksforgeeks.org/partition-problem-dp-18/)\n",
    "\n",
    "Partition problem is to determine whether a given set can be partitioned into two subsets such that the sum of elements in both subsets is the same. \n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "arr[] = {1, 5, 11, 5}\n",
    "\n",
    "Output: true \n",
    "\n",
    "The array can be partitioned as {1, 5, 5} and {11}"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def equal_sum_partition(arr):\n",
    "    sum_arr = sum(arr)\n",
    "    if sum_arr % 2 != 0:\n",
    "        return False\n",
    "    else:\n",
    "        s = sum_arr // 2\n",
    "        n = len(arr)\n",
    "        t = [[False for _ in range(s+1)] for _ in range(n+1)]\n",
    "        for i in range(n+1):\n",
    "            for j in range(s+1):\n",
    "                if i == 0:\n",
    "                    t[i][j] = False\n",
    "                    t[0][0] = True\n",
    "                elif j == 0:\n",
    "                    t[i][j] = True\n",
    "                    \n",
    "                elif arr[i-1] <= j:\n",
    "                    t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]\n",
    "                else:\n",
    "                    t[i][j] = t[i-1][j]\n",
    "    \n",
    "        return t[n][s]\n",
    "    \n",
    "equal_sum_partition([1, 5, 11, 5])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Subset with sum divisible by m](https://www.geeksforgeeks.org/subset-sum-divisible-m/)\n",
    "\n",
    "Given a set of non-negative distinct integers, and a value m, determine if there is a subset of the given set with sum divisible by m.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input : arr = [3, 1, 7, 5], m = 6\n",
    "\n",
    "Output : True\n",
    "\n",
    "Input : arr = [1, 6], m = 5\n",
    "\n",
    "Output : NO"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 1,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def subset_with_sum_div_m(arr, m):\n",
    "    n = len(arr)\n",
    "\n",
    "    dp = [False for _ in range(m)]\n",
    "\n",
    "    for i in range(n):\n",
    "        temp = [False for _ in range(m)]\n",
    "        for j in range(m):\n",
    "            if dp[j] == True and dp[(j+arr[i]) % m] == False:\n",
    "                temp[(j+arr[i]) % m] = True\n",
    "\n",
    "        for j in range(m):\n",
    "            if temp[j] == True:\n",
    "                dp[j] = True\n",
    "\n",
    "        dp[arr[i]%m] = True\n",
    "\n",
    "    return dp[0]\n",
    "    \n",
    "subset_with_sum_div_m([3, 1, 7, 5], 6)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Count of Subsets of a given Sum](https://www.geeksforgeeks.org/count-of-subsets-with-sum-equal-to-x/)\n",
    "\n",
    "Number of subsets in a given array whose sum is equal to given value of sum.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input : arr[] : [2, 3, 5, 6, 8, 10], s = 10\n",
    "\n",
    "Output : 3\n",
    "\n",
    "Explanation : Three subsets [2, 3, 5], [2, 8], [10] exists whose sum is equal to s."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def count_of_subsets_of_given_sum(arr, s):\n",
    "    n = len(arr)\n",
    "    t = [[-1 for _ in range(s+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if i == 0:\n",
    "                t[i][j] = 0\n",
    "                t[0][0] = 1\n",
    "            elif j == 0:\n",
    "                t[i][j] = 1\n",
    "            elif arr[i-1] <= j:\n",
    "                t[i][j] = t[i-1][j-arr[i-1]] + t[i-1][j]\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return t[n][s]\n",
    "\n",
    "count_of_subsets_of_given_sum([2, 3, 5, 6, 8, 10], 10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Minimum Subset Sum Difference](https://www.geeksforgeeks.org/partition-a-set-into-two-subsets-such-that-the-difference-of-subset-sums-is-minimum/)\n",
    "\n",
    "Given a set of integers, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums is minimum. \n",
    "\n",
    "If there is a set S with n elements, then if we assume Subset1 has m elements, Subset2 must have n-m elements and the value of abs(sum(Subset1) – sum(Subset2)) should be minimum.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input:  arr[] = {1, 6, 11, 5} \n",
    "\n",
    "Output: 1\n",
    "\n",
    "Explanation:\n",
    "\n",
    "Subset1 = {1, 5, 6}, sum of Subset1 = 12 \n",
    "\n",
    "Subset2 = {11}, sum of Subset2 = 11    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def subset_sum(arr, s):\n",
    "    n = len(arr)\n",
    "    t = [[False for _ in range(s+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if i == 0:\n",
    "                t[i][j] = False\n",
    "                t[0][0] = True\n",
    "            elif j == 0:\n",
    "                t[i][j] = True\n",
    "            elif arr[i-1] <= j:\n",
    "                t[i][j] = t[i-1][j-arr[i-1]] or t[i-1][j]\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return [i for i in range(s // 2 + 1) if t[-1][i]]\n",
    "\n",
    "def minimum_subset_sum_difference(arr):\n",
    "    s = sum(arr)\n",
    "    v = subset_sum(arr, s)\n",
    "    mn = float('inf')\n",
    "    \n",
    "    for i in v:\n",
    "        mn = min(mn, s - 2*i)\n",
    "    return mn\n",
    "\n",
    "minimum_subset_sum_difference([1, 5, 6, 11])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Count the number of subset with a given difference\n",
    "\n",
    "Given a set of integers, the task is to divide it into two sets S1 and S2 such that the absolute difference between their sums should be equal to given value of s. \n",
    "\n",
    "If there is a set S with n elements, then if we assume Subset1 has m elements, Subset2 must have n-m elements and the value of abs(sum(Subset1) – sum(Subset2)) should be equal to given value of s.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input:  arr[] = [1, 1, 2, 3], s = 1\n",
    "\n",
    "Output: 3"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def count_subset_sum(arr, s):\n",
    "    n = len(arr)\n",
    "    t = [[0 for _ in range(s + 1)] for _ in range(n + 1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if i == 0:\n",
    "                t[i][j] = 0\n",
    "                t[0][0] = 1\n",
    "            elif j == 0:\n",
    "                t[i][j] = 1\n",
    "            elif arr[i-1] <= j:\n",
    "                t[i][j] = t[i-1][j-arr[i-1]] + t[i-1][j]\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return t[n][s]\n",
    "\n",
    "def num_subset_given_diff(arr, diff):\n",
    "    s_arr = sum(arr)\n",
    "    sum_subset1 = (s_arr + diff) // 2\n",
    "    \n",
    "    count = count_subset_sum(arr, sum_subset1)\n",
    "    \n",
    "    return count\n",
    "\n",
    "num_subset_given_diff([1, 1, 2, 3], 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Target Sum (same as num_subset_given_diff)\n",
    "\n",
    "You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.\n",
    "\n",
    "Find out how many ways to assign symbols to make sum of integers equal to target S.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input: nums is [1, 1, 1, 1, -1], S is 3. \n",
    "\n",
    "Output: 5\n",
    "\n",
    "Explanation: \n",
    "\n",
    "-1+1+1+1+1 = 3\n",
    "\n",
    "+1-1+1+1+1 = 3\n",
    "\n",
    "+1+1-1+1+1 = 3\n",
    "\n",
    "+1+1+1-1+1 = 3\n",
    "\n",
    "+1+1+1+1-1 = 3\n",
    "\n",
    "There are 5 ways to assign symbols to make the sum of nums be target 3."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def target_sum(arr, s):\n",
    "    return num_subset_given_diff(arr, s)\n",
    "\n",
    "target_sum([1, 1, 2, 3], 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Unbounded Knapsack](https://www.geeksforgeeks.org/unbounded-knapsack-repetition-items-allowed/)\n",
    "\n",
    "Given a knapsack weight W and a set of n items with certain value vali and weight wti, we need to calculate the maximum amount that could make up this quantity exactly. This is different from classical Knapsack problem, here we are allowed to use unlimited number of instances of an item.\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [],
   "source": [
    "wt = [1, 2, 3, 4, 5, 6]\n",
    "val = [6, 1, 9, 4, 2, 8]\n",
    "w = 7"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "42"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def unbounded_knapsack(wt,  val, w):\n",
    "    n = len(wt)\n",
    "    t = [[0 for _ in range(w+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(w+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            if wt[i-1] <= j:\n",
    "                t[i][j] = max(val[i-1] + t[i][j-wt[i-1]], t[i-1][j])\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "                \n",
    "    return t[n][w]\n",
    "\n",
    "unbounded_knapsack(wt, val, w)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Rod Cutting Problem](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/)\n",
    "\n",
    "Given a rod of length n inches and an array of prices that contains prices of all pieces of size smaller than n. Determine the maximum value obtainable by cutting up the rod and selling the pieces. For example, if length of the rod is 8 and the values of different pieces are given as following, then the maximum obtainable value is 22 (by cutting in two pieces of lengths 2 and 6).\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input:\n",
    "\n",
    "length[] = [1, 2, 3, 4, 5, 6, 7, 8]\n",
    "\n",
    "price [] = [1, 5, 8, 9, 10, 17, 17, 20]\n",
    " \n",
    "Rod length: 4\n",
    "\n",
    "Output:\n",
    "\n",
    "Cut the rod into two pieces of length 2 each to gain revenue of 5 + 5 = 10"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "22"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def rod_cutting_problem(length, price, max_len):\n",
    "    n = len(length)\n",
    "    t = [[0 for _ in range(max_len+1)] for _ in range(n + 1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(max_len+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif length[i-1] <= j:\n",
    "                t[i][j] = max(price[i-1] + t[i][j - length[i-1]], t[i-1][j])\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "                \n",
    "    return t[n][max_len]\n",
    "\n",
    "rod_cutting_problem([1, 2, 3, 4, 5, 6, 7, 8], [1, 5, 8, 9, 10, 17, 17, 20], 8)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Maximize the number of segments of length p, q and r](https://www.geeksforgeeks.org/maximize-the-number-of-segments-of-length-p-q-and-r/)\n",
    "\n",
    "Given a rod of length L, the task is to cut the rod in such a way that the total number of segments of length p, q and r is maximized. The segments can only be of length p, q, and r.\n",
    "\n",
    "#### Input / Output\n",
    "\n",
    "input: l = 11, p = 2, q = 3, r = 5\n",
    "\n",
    "Output: 5 \n",
    "\n",
    "Segments of 2, 2, 2, 2 and 3\n",
    "\n",
    "Input: l = 7, p = 2, q = 5, r = 5 \n",
    "\n",
    "Output: 2 \n",
    "\n",
    "Segments of 2 and 5"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def maximizeTheCuts(l,p,q,r):\n",
    "    dp = [-1 for _ in range(l+1)]\n",
    "    dp[0] = 0\n",
    "\n",
    "    for i in range(l+1):\n",
    "        if dp[i] == -1:\n",
    "            continue\n",
    "\n",
    "        if i+p <=l:\n",
    "            dp[i+p] = max(dp[i+p], dp[i]+1)\n",
    "        if i+q <=l:\n",
    "            dp[i+q] = max(dp[i+q], dp[i]+1)\n",
    "        if i+r <=l:\n",
    "            dp[i+r] = max(dp[i+r], dp[i]+1)\n",
    "\n",
    "    return dp[l] if dp[l] != -1 else 0\n",
    "\n",
    "maximizeTheCuts(7, 2, 5, 5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Coin change problem : Maximum number of ways](https://www.geeksforgeeks.org/coin-change-dp-7/)\n",
    "\n",
    "Given a value N, if we want to make change for N cents, and we have infinite supply of each of S = { S1, S2, .. , Sm} valued coins, how many ways can we make the change? The order of coins doesn’t matter.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input: amount = 5, coins = [1, 2, 5]\n",
    "\n",
    "Output: 4\n",
    "\n",
    "Explanation: there are four ways to make up the amount:\n",
    "\n",
    "5=5\n",
    "\n",
    "5=2+2+1\n",
    "\n",
    "5=2+1+1+1\n",
    "\n",
    "5=1+1+1+1+1"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "7"
      ]
     },
     "execution_count": 15,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def coin_change_1(coins, s):\n",
    "    n = len(coins)\n",
    "    t = [[0 for _ in range(s + 1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if j == 0:\n",
    "                t[i][j] = 1\n",
    "                t[0][0] = 0\n",
    "            elif coins[i-1] <= j:\n",
    "                t[i][j] = t[i][j-coins[i-1]] + t[i-1][j]\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return t[n][s]\n",
    "\n",
    "coin_change_1([1, 3, 5], 10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Coin change problem : Minimum number of coins](https://www.geeksforgeeks.org/find-minimum-number-of-coins-that-make-a-change/)\n",
    "\n",
    "You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input: coins = [1,2,5], amount = 11\n",
    "\n",
    "Output: 3\n",
    "\n",
    "Explanation: 11 = 5 + 5 + 1"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import math\n",
    "\n",
    "def coin_change_2(coins, s):\n",
    "    n = len(coins)\n",
    "    t = [[-1 for _ in range(s+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(s+1):\n",
    "            if i == 0:\n",
    "                t[i][j] = math.inf\n",
    "            elif j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif i == 1:\n",
    "                if j%coins[i-1]==0:\n",
    "                    t[i][j] = j // coins[i-1]\n",
    "                else:\n",
    "                    t[i][j] = math.inf\n",
    "            elif coins[i-1] <= j:\n",
    "                t[i][j] = min(1+t[i][j-coins[i-1]], t[i-1][j])\n",
    "            else:\n",
    "                t[i][j] = t[i-1][j]\n",
    "    \n",
    "    return t[n][s]\n",
    "\n",
    "coin_change_2([1, 2, 3], 5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Longest common subsequence](https://www.geeksforgeeks.org/longest-common-subsequence-dp-4/)\n",
    "\n",
    "LCS Problem Statement: Given two sequences, find the length of longest subsequence present in both of them. A subsequence is a sequence that appears in the same relative order, but not necessarily contiguous. For example, “abc”, “abg”, “bdf”, “aeg”, ‘”acefg”, .. etc are subsequences of “abcdefg”.\n",
    "\n",
    "In order to find out the complexity of brute force approach, we need to first know the number of possible different subsequences of a string with length n, i.e., find the number of subsequences with lengths ranging from 1,2,..n-1. Recall from theory of permutation and combination that number of combinations with 1 element are nC1. Number of combinations with 2 elements are nC2 and so forth and so on. We know that nC0 + nC1 + nC2 + … nCn = 2n. So a string of length n has 2n-1 different possible subsequences since we do not consider the subsequence with length 0. This implies that the time complexity of the brute force approach will be O(n * 2n). Note that it takes O(n) time to check if a subsequence is common to both the strings. This time complexity can be improved using dynamic programming.\n",
    "\n",
    "```\n",
    "Examples:\n",
    "LCS for input Sequences “ABCDGH” and “AEDFHR” is “ADH” of length 3.\n",
    "LCS for input Sequences “AGGTAB” and “GXTXAYB” is “GTAB” of length 4.\n",
    "```\n",
    " \n",
    " ### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [],
   "source": [
    "X,Y = 'abcdgh', 'abcdfghr'\n",
    "lenX, lenY = len(X), len(Y)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Using Recursion"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "6"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def longest_common_subsequence_rec(X, Y, lenX, lenY):\n",
    "    if lenX == 0 or lenY == 0:\n",
    "        return 0\n",
    "    elif X[lenX-1] == Y[lenY-1]:\n",
    "        return 1 + longest_common_subsequence_rec(X, Y, lenX-1, lenY-1)\n",
    "    else:\n",
    "        return max(longest_common_subsequence_rec(X, Y, lenX-1, lenY), \n",
    "                   longest_common_subsequence_rec(X, Y, lenX, lenY-1))\n",
    "    \n",
    "longest_common_subsequence_rec(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Memomization"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "6"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "def longest_common_subsequence_memo(X, Y, lenX, lenY):\n",
    "    if lenX == 0 or lenY == 0:\n",
    "        t[lenX][lenY] = 0\n",
    "    if t[lenX][lenY] != -1:\n",
    "        pass\n",
    "    elif X[lenX-1] == Y[lenY-1]:\n",
    "        t[lenX][lenY] = 1 + longest_common_subsequence_memo(X, Y, lenX-1, lenY-1)\n",
    "    else:\n",
    "        t[lenX][lenY] = max(longest_common_subsequence_memo(X, Y, lenX-1, lenY),\n",
    "                            longest_common_subsequence_memo(X, Y, lenX, lenY-1))\n",
    "    \n",
    "    return t[lenX][lenY]\n",
    "\n",
    "longest_common_subsequence_memo(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Top Down Approach"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "6"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def longest_common_subsequence_top_down(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif X[i-1] == Y[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "    \n",
    "    return t[lenX][lenY]\n",
    "\n",
    "longest_common_subsequence_top_down(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Longest Common Substring](https://www.geeksforgeeks.org/longest-common-substring-dp-29/)\n",
    "\n",
    "Given two strings ‘X’ and ‘Y’, find the length of the longest common substring.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input : X = “GeeksforGeeks”, y = “GeeksQuiz” \n",
    "\n",
    "Output : 5 \n",
    "\n",
    "Explanation:\n",
    "\n",
    "The longest common substring is “Geeks” and is of length 5."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "4"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def longest_common_substring(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    longest_common = 0\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif X[i-1] == Y[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                longest_common = max(longest_common, t[i][j])\n",
    "            else:\n",
    "                t[i][j] = 0\n",
    "    \n",
    "    return longest_common\n",
    "\n",
    "longest_common_substring(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Print Longest Common subsequence](https://www.geeksforgeeks.org/printing-longest-common-subsequence/)\n",
    "\n",
    "Given two sequences, print the longest subsequence present in both of them.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "LCS for input Sequences “ABCDGH” and “AEDFHR” is “ADH” of length 3."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'abcdgh'"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def print_longest_common_subsequence(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            if X[i-1] == Y[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i][j-1], t[i-1][j])\n",
    "    \n",
    "    i = lenX\n",
    "    j = lenY \n",
    "    longest_common = ''\n",
    "    while i != 0 or j != 0:\n",
    "        if X[i-1] == Y[j-1]:\n",
    "            longest_common = X[i-1] + longest_common\n",
    "            i -= 1\n",
    "            j -= 1\n",
    "        else:\n",
    "            if t[i][j-1] > t[i-1][j]:\n",
    "                j -= 1\n",
    "            else:\n",
    "                i -= 1\n",
    "                \n",
    "    return longest_common\n",
    "\n",
    "print_longest_common_subsequence(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Print Longest common substring](https://www.geeksforgeeks.org/print-longest-common-substring/)\n",
    "\n",
    "Given two sequences, print the longest substring present in both of them."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'abcd'"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def print_longest_common_substring(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    longest_common = ''\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if  i == 0 or j == 0:\n",
    "                t[i][j] = ''\n",
    "            elif X[i-1] == Y[j-1]:\n",
    "                t[i][j] = t[i-1][j-1] + X[i-1]\n",
    "                \n",
    "                if len(t[i][j]) > len(longest_common):\n",
    "                    longest_common = t[i][j]\n",
    "            else:\n",
    "                t[i][j] = ''\n",
    "        \n",
    "    return longest_common\n",
    "\n",
    "print_longest_common_substring(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Shortest Common SuperSubsequence](https://www.geeksforgeeks.org/shortest-common-supersequence/)\n",
    "\n",
    "Given two strings str1 and str2, the task is to find the length of the shortest string that has both str1 and str2 as subsequences.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input:   str1 = \"geek\",  str2 = \"eke\"\n",
    "\n",
    "Output: 5\n",
    "\n",
    "Explanation: \n",
    "\n",
    "String \"geeke\" has both string \"geek\" \n",
    "and \"eke\" as subsequences."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "10"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def shortest_common_super_subsequence(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    \n",
    "    longest_common = -1\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif X[i-1] == Y[i-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                if longest_common < t[i][j]:\n",
    "                    longest_common = t[i][j]\n",
    "            else:\n",
    "                t[i][j] = 0\n",
    "    \n",
    "    return lenX + lenY - longest_common\n",
    "\n",
    "shortest_common_super_subsequence(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Minimum number of insertion and deletion to convert a string X to Y](https://www.geeksforgeeks.org/minimum-number-deletions-insertions-transform-one-string-another/)\n",
    "\n",
    "Given two strings ‘str1’ and ‘str2’ of size m and n respectively. The task is to remove/delete and insert the minimum number of characters from/in str1 to transform it into str2. It could be possible that the same character needs to be removed/deleted from one point of str1 and inserted to some another point.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input : \n",
    "\n",
    "str1 = \"heap\", str2 = \"pea\" \n",
    "\n",
    "Output : \n",
    "\n",
    "Minimum Deletion = 2 and\n",
    "\n",
    "Minimum Insertion = 1\n",
    "\n",
    "Explanation:\n",
    "\n",
    "p and h deleted from heap\n",
    "Then, p is inserted at the beginning\n",
    "One thing to note, though p was required yet\n",
    "it was removed/deleted first from its position and\n",
    "then it is inserted to some other position.\n",
    "Thus, p contributes one to the deletion_count\n",
    "and one to the insertion_count.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "ename": "NameError",
     "evalue": "name 'X' is not defined",
     "output_type": "error",
     "traceback": [
      "\u001b[0;31m---------------------------------------------------------------------------\u001b[0m",
      "\u001b[0;31mNameError\u001b[0m                                 Traceback (most recent call last)",
      "\u001b[0;32m<ipython-input-1-f74ed70fca85>\u001b[0m in \u001b[0;36m<module>\u001b[0;34m\u001b[0m\n\u001b[1;32m     23\u001b[0m     \u001b[0;32mreturn\u001b[0m \u001b[0mnum_del\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mnum_insrt\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[1;32m     24\u001b[0m \u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0;32m---> 25\u001b[0;31m \u001b[0mmin_insrt_del\u001b[0m\u001b[0;34m(\u001b[0m\u001b[0mX\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mY\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mlenX\u001b[0m\u001b[0;34m,\u001b[0m \u001b[0mlenY\u001b[0m\u001b[0;34m)\u001b[0m\u001b[0;34m\u001b[0m\u001b[0;34m\u001b[0m\u001b[0m\n\u001b[0m",
      "\u001b[0;31mNameError\u001b[0m: name 'X' is not defined"
     ]
    }
   ],
   "source": [
    "def min_insrt_del(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range (lenX+1)]\n",
    "\n",
    "    num_insrt = 0\n",
    "    num_del = 0\n",
    "    \n",
    "    longest_common = 0\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif X[i-1] == Y[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                if t[i][j] > longest_common:\n",
    "                    longest_common = t[i][j]\n",
    "            else:\n",
    "                t[i][j] = max(t[i][j-1], t[i-1][j])\n",
    "                \n",
    "    num_del = lenX - longest_common\n",
    "    num_insrt = lenY - longest_common\n",
    "    \n",
    "    return num_del, num_insrt\n",
    "\n",
    "min_insrt_del(X, Y, lenX, lenY)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Longest Palindromic Subsequence](https://www.geeksforgeeks.org/longest-palindromic-subsequence-dp-12/)\n",
    "\n",
    "Given a sequence, find the length of the longest palindromic subsequence in it.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input: string = \"cbbd\"\n",
    "\n",
    "Output: 2\n",
    "\n",
    "Explaination:\n",
    "\n",
    "One possible longest palindromic subsequence is \"bb\"."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "5"
      ]
     },
     "execution_count": 26,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def longest_palindromic_subsequence(string):\n",
    "    n = len(string)\n",
    "    rev_string = string[::-1]\n",
    "    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]\n",
    "    \n",
    "    longest_palindromic = 0\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(n+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif string[i-1] == rev_string[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                if t[i][j] > longest_palindromic:\n",
    "                    longest_palindromic = t[i][j]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])                \n",
    "    \n",
    "    return longest_palindromic\n",
    "\n",
    "longest_palindromic_subsequence('agbcba')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Minimum number of deletion to make a string palindrome](https://www.geeksforgeeks.org/minimum-number-deletions-make-string-palindrome/)\n",
    "\n",
    "Given a string of size ‘n’. The task is to remove or delete the minimum number of characters from the string so that the resultant string is a palindrome. \n",
    "\n",
    "Note: The order of characters should be maintained.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input : aebcbda\n",
    "\n",
    "Output : 2\n",
    "\n",
    "Remove characters 'e' and 'd'\n",
    "Resultant string will be 'abcba'\n",
    "which is a palindromic string"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def min_del_to_palindrome(string):\n",
    "    n = len(string)\n",
    "    rev_string = string[::-1]\n",
    "    \n",
    "    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]\n",
    "    \n",
    "    longest_common = 0\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(n+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif string[i-1] == rev_string[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                if t[i][j] > longest_common:\n",
    "                    longest_common = t[i][j]\n",
    "                    \n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "    \n",
    "    return n - longest_common\n",
    "\n",
    "min_del_to_palindrome('agbcba')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Print Shortest Common SuperSequence](https://www.geeksforgeeks.org/print-shortest-common-supersequence/)\n",
    "\n",
    "Given two strings X and Y, print the shortest string that has both X and Y as subsequences. If multiple shortest supersequence exists, print any one of them.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "\n",
    "Input: X = \"AGGTAB\",  Y = \"GXTXAYB\"\n",
    "\n",
    "Output: \"AGXGTXAYB\" OR \"AGGXTXAYB\" OR Any string that represents shortest\n",
    "supersequence of X and Y"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'acbcdaf'"
      ]
     },
     "execution_count": 28,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def print_shortest_super_sequence(X, Y, lenX, lenY):\n",
    "    t = [[-1 for _ in range(lenY+1)] for _ in range(lenX+1)]\n",
    "    \n",
    "    result = ''\n",
    "    \n",
    "    for i in range(lenX+1):\n",
    "        for j in range(lenY+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif X[i-1] == Y[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "    \n",
    "    i = lenX\n",
    "    j = lenY\n",
    "    \n",
    "    while i != 0 or j != 0:\n",
    "        if X[i-1] == Y[j-1]:\n",
    "            result = X[i-1] + result\n",
    "            i -= 1\n",
    "            j -= 1\n",
    "        elif t[i][j-1] > t[i-1][j]:\n",
    "            result = Y[j-1] + result\n",
    "            j -= 1\n",
    "        else:\n",
    "            result = X[i-1] + result\n",
    "            i -= 1\n",
    "\n",
    "    return result\n",
    "\n",
    "print_shortest_super_sequence('acbcf', 'abcdaf', 5, 6)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Longest Repeating SubSequence](https://www.geeksforgeeks.org/longest-repeating-subsequence/)\n",
    "\n",
    "Given a string, find length of the longest repeating subseequence such that the two subsequence don’t have same string character at same position, i.e., any i’th character in the two subsequences shouldn’t have the same index in the original string.\n",
    "\n",
    "#### Input/Output\n",
    "\n",
    "Input: str = \"aab\"\n",
    "\n",
    "\n",
    "Output: 1\n",
    "\n",
    "The two subssequence are 'a'(first) and 'a'(second). \n",
    "Note that 'b' cannot be considered as part of subsequence \n",
    "as it would be at same index in both."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "('ABD', 3)"
      ]
     },
     "execution_count": 29,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def longest_repeating_subsequence(string):\n",
    "    n = len(string)\n",
    "    \n",
    "    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]\n",
    "    \n",
    "    result = ''\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(n+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif string[i-1] == string[j-1] and i != j:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "                if string[i-1] not in result:\n",
    "                    result = result + string[i-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "    \n",
    "    return result, t[n][n]\n",
    "\n",
    "longest_repeating_subsequence('AABEBCDD')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Sequence Pattern Matching](https://www.geeksforgeeks.org/dynamic-programming-wildcard-pattern-matching-linear-time-constant-space/)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def sequence_pattern_matching(a, b):\n",
    "    len_a = len(a)\n",
    "    len_b = len(b)\n",
    "\n",
    "    t = [[-1 for _ in range(len_b+1)] for _ in range(len_a+1)]\n",
    "    \n",
    "    for i in range(len_a+1):\n",
    "        for j in range(len_b+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif a[i-1] == b[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "                \n",
    "    return t[len_a][len_b] == len_a or t[len_a][len_b] == len_b\n",
    "\n",
    "sequence_pattern_matching('AXY', 'ADXCPY')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Minimum number of insertion to make a string palindrome](https://www.geeksforgeeks.org/minimum-insertions-to-form-a-palindrome-dp-28/)\n",
    "#### (number of insertion == number of deletion)\n",
    "\n",
    "Given a string str, the task is to find the minimum number of characters to be inserted to convert it to palindrome.\n",
    "\n",
    "Example:\n",
    "\n",
    "ab: Number of insertions required is 1 i.e. bab\n",
    "\n",
    "aa: Number of insertions required is 0 i.e. aa"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "2"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def min_insrt_palindrome(string):\n",
    "    n = len(string)\n",
    "    rev_string = string[::-1]\n",
    "    \n",
    "    t = [[-1 for _ in range(n+1)] for _ in range(n+1)]\n",
    "    \n",
    "    for i in range(n+1):\n",
    "        for j in range(n+1):\n",
    "            if i == 0 or j == 0:\n",
    "                t[i][j] = 0\n",
    "            elif string[i-1] == rev_string[j-1]:\n",
    "                t[i][j] = 1 + t[i-1][j-1]\n",
    "            else:\n",
    "                t[i][j] = max(t[i-1][j], t[i][j-1])\n",
    "                \n",
    "    return n - t[n][n]\n",
    "\n",
    "min_insrt_palindrome('aebcbda')"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Matrix Chain Multiplication](https://www.geeksforgeeks.org/matrix-chain-multiplication-dp-8/)\n",
    "\n",
    "Given a sequence of matrices, find the most efficient way to multiply these matrices together. The problem is not actually to perform the multiplications, but merely to decide in which order to perform the multiplications.\n",
    "\n",
    "We have many options to multiply a chain of matrices because matrix multiplication is associative. In other words, no matter how we parenthesize the product, the result will be the same. For example, if we had four matrices A, B, C, and D, we would have: \n",
    "\n",
    "(ABC)D = (AB)(CD) = A(BCD) = ....\n",
    "\n",
    "However, the order in which we parenthesize the product affects the number of simple arithmetic operations needed to compute the product, or the efficiency. For example, suppose A is a 10 × 30 matrix, B is a 30 × 5 matrix, and C is a 5 × 60 matrix. Then,  \n",
    "\n",
    "(AB)C = (10×30×5) + (10×5×60) = 1500 + 3000 = 4500 operations\n",
    "A(BC) = (30×5×60) + (10×30×60) = 9000 + 18000 = 27000 operations.\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [],
   "source": [
    "arr = [40, 20, 30, 10, 30]\n",
    "n = len(arr)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Recursive Approach"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "26000"
      ]
     },
     "execution_count": 33,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import math\n",
    "\n",
    "def matrix_chain_multiplication(arr, i ,j):\n",
    "    if i >= j:\n",
    "        return 0\n",
    "    \n",
    "    ans = math.inf\n",
    "\n",
    "    for k in range(i, j):\n",
    "        before = matrix_chain_multiplication(arr, i, k)\n",
    "        after = matrix_chain_multiplication(arr, k+1, j)\n",
    "        temp = before + after + arr[i-1] * arr[k] * arr[j]\n",
    "        if temp < ans:\n",
    "            ans = temp\n",
    "    \n",
    "    return ans        \n",
    "\n",
    "matrix_chain_multiplication(arr, 1, n-1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Matrix Chain Multiplication Memomization"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "26000"
      ]
     },
     "execution_count": 34,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(n)] for _ in range(n)]\n",
    "def matrix_chain_multiplication_memo(arr, i, j):\n",
    "    if i >= j:\n",
    "        return 0\n",
    "    if t[i][j] != -1:\n",
    "        return t[i][j]\n",
    "    \n",
    "    temp = math.inf\n",
    "    for k in range(i, j):\n",
    "        before = matrix_chain_multiplication_memo(arr, i, k)\n",
    "        after = matrix_chain_multiplication_memo(arr, k+1, j)\n",
    "        temp = min(temp, before+after+arr[i-1]*arr[k]*arr[j])\n",
    "    \n",
    "    t[i][j] = temp\n",
    "    return temp\n",
    "\n",
    "matrix_chain_multiplication_memo(arr, 1, n-1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Palindrome Partitioning](https://www.geeksforgeeks.org/palindrome-partitioning-dp-17/)\n",
    "\n",
    "Given a string, a partitioning of the string is a palindrome partitioning if every substring of the partition is a palindrome. For example, “aba|b|bbabb|a|b|aba” is a palindrome partitioning of “ababbbabbababa”. Determine the fewest cuts needed for a palindrome partitioning of a given string. For example, minimum of 3 cuts are needed for “ababbbabbababa”. The three cuts are “a|babbbab|b|ababa”. If a string is a palindrome, then minimum 0 cuts are needed. If a string of length n containing all different characters, then minimum n-1 cuts are needed.\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "import math\n",
    "string = \"abbab\"\n",
    "n = len(string)\n",
    "i = 0\n",
    "j = n - 1"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Palindrome Partitioning Recursive"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def is_palindrome(string):\n",
    "    if len(string) == 1:\n",
    "        return True\n",
    "    elif string == string[::-1]:\n",
    "        return True\n",
    "    return False\n",
    "\n",
    "def palindrome_partition(string, i, j):\n",
    "    if i >= j:\n",
    "        return 0\n",
    "    if is_palindrome(string[i:j+1]):\n",
    "        return 0\n",
    "    \n",
    "    max_ = math.inf\n",
    "    for k in range(i, j):\n",
    "        before = palindrome_partition(string, i, k)\n",
    "        after = palindrome_partition(string, k+1, j)\n",
    "        temp = before + after + 1\n",
    "        if temp < max_:\n",
    "            max_ = temp\n",
    "        \n",
    "    return max_\n",
    "\n",
    "palindrome_partition(string, i, j)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Palindrome Partitioning Memomization"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(n)] for _ in range(n)]\n",
    "def palindrome_partition_memo(string, i, j):\n",
    "    if i >= j:\n",
    "        return 0\n",
    "    if is_palindrome(string[i:j+1]):\n",
    "        return 0\n",
    "    if t[i][j] != -1:\n",
    "        return t[i][j]\n",
    "    \n",
    "    temp = math.inf\n",
    "    for k in range(i, j):\n",
    "        before = palindrome_partition_memo(string, i, k)\n",
    "        after  = palindrome_partition_memo(string, k+1, j)\n",
    "        temp = min(temp, before+after+1)\n",
    "    \n",
    "    t[i][j] = temp\n",
    "    return t[i][j]\n",
    "\n",
    "palindrome_partition_memo(string, i, j)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Optimaization\n",
    "\n",
    "Optimizations: We can store our answer of isPalindrome(s, i, j) in a dp state (i, j). Note that every recursive call for the left and right parts of the substring stop when the base case (i.e the partitioned string is a palindrome) is reached. The major optimization in this question is that we partition the string only when the left part i.e. s[i...k] is a palindrome. Doing this we're effectively reducing a lot of unnecessary checks for every k."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(n+1)] for _ in range(n+1)]\n",
    "def palindrome_partition_opt(string, i, j):\n",
    "    if i >= j:\n",
    "        return 0\n",
    "    if is_palindrome(string[i:j+1]):\n",
    "        return 0\n",
    "    \n",
    "    temp = math.inf\n",
    "    for k in range(i, j):\n",
    "        # Optimization \n",
    "        if not self.is_palindrome(string[i:k+1]):\n",
    "            continue\n",
    "        if t[i][k] != -1:\n",
    "            left = t[i][k]\n",
    "        else:\n",
    "            left = palindrome_partition_opt(string, i, k)\n",
    "        if t[k+1][j] != -1:\n",
    "            right = t[k+1][j]\n",
    "        else:\n",
    "            right = palindrome_partition_opt(string, k+1, j)\n",
    "            \n",
    "        temp = min(temp, left+right+1)\n",
    "        \n",
    "    t[i][j] = temp\n",
    "    \n",
    "    return t[i][j]\n",
    "\n",
    "palindrome_partition_opt(string, i, j)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Evaluate expression to True (Boolean Parenthisization)](https://www.geeksforgeeks.org/boolean-parenthesization-problem-dp-37/)\n",
    "\n",
    "Given a boolean expression with following symbols. \n",
    "\n",
    "Symbols\n",
    "\n",
    " - 'T' ---> true \n",
    " -  'F' ---> false \n",
    "\n",
    "And following operators filled between symbols \n",
    "\n",
    "Operators\n",
    " -    &   ---> boolean AND\n",
    " -   |   ---> boolean OR\n",
    " -   ^   ---> boolean XOR \n",
    "\n",
    "Count the number of ways we can parenthesize the expression so that the value of expression evaluates to true. \n",
    "\n",
    "#### Input"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [],
   "source": [
    "string = 'T|F&T^F'\n",
    "n = len(string)\n",
    "i = 0\n",
    "j = n-1"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Recursive Method"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "4"
      ]
     },
     "execution_count": 40,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def eval_expr_true(string, i, j, isTrue=True):\n",
    "    if i > j:\n",
    "        return False\n",
    "    if i == j:\n",
    "        if isTrue == True:\n",
    "            return string[i] == 'T'\n",
    "        else:\n",
    "            return string[i] == 'F'\n",
    "        \n",
    "    ans = 0\n",
    "    \n",
    "    for k in range(i+1, j):\n",
    "        if string[k] in '|&^':\n",
    "            lt = eval_expr_true(string, i, k-1, True)\n",
    "            lf = eval_expr_true(string, i, k-1, False)\n",
    "            rt = eval_expr_true(string, k+1, j, True)\n",
    "            rf = eval_expr_true(string, k+1, j, False)\n",
    "            \n",
    "            if string[k] == '|':\n",
    "                temp = (lt and rt) + (lf and rt) + (lt and rf) if isTrue else (lf and rf)\n",
    "            elif string[k] == '&':\n",
    "                temp = (lt and rt) if isTrue else (lf and rf) + (lf and rt) + (lt and rf)\n",
    "            elif string[k] == '^':\n",
    "                temp = (lt and rf) + (lf and rt) if isTrue else (lf and rf) + (lt and rt)\n",
    "            \n",
    "            ans += temp\n",
    "    return ans\n",
    "\n",
    "eval_expr_true(string, i, j)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Memomization Method"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "4"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[[-1 for _ in range(n)] for _ in range(n)] for _ in range(2)]\n",
    "def eval_expr_true_memo(string, i, j, isTrue=True):\n",
    "    if i > j:\n",
    "        return 0\n",
    "    if i == j:\n",
    "        return string[i] == 'T' if isTrue else string[i] == 'F'\n",
    "    \n",
    "    T = 1 if isTrue else 0\n",
    "    if t[T][i][j] != -1:\n",
    "        return t[T][i][j]\n",
    "\n",
    "    ans = 0\n",
    "    \n",
    "    for k in range(i+1, j):\n",
    "        if string[k] in '|&^':\n",
    "            lt = eval_expr_true_memo(string, i, k-1, True)\n",
    "            lf = eval_expr_true_memo(string, i, k-1, False)\n",
    "            rt = eval_expr_true_memo(string, k+1, j, True)\n",
    "            rf = eval_expr_true_memo(string, k+1, j, False)\n",
    "            \n",
    "            if string[k] == '|':\n",
    "                if isTrue:\n",
    "                    temp = (lt and rt) + (lf and rt) + (lt and rf)\n",
    "                else:\n",
    "                    temp = lf and rf\n",
    "            elif string[k] == '&':\n",
    "                if isTrue:\n",
    "                    temp = (lt and rt)\n",
    "                else:\n",
    "                    temp = (lf and rf) + (lf and rt) + (lt and rf)\n",
    "            elif string[k] == '^':\n",
    "                if isTrue:\n",
    "                    temp = (lt and rf) + (lf and rt)\n",
    "                else:\n",
    "                    temp = (lf and rf) + (lt and rt)\n",
    "            \n",
    "            ans += temp\n",
    "            \n",
    "    t[T][i][j] = ans\n",
    "    return ans\n",
    "\n",
    "eval_expr_true_memo(string, i, j)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Scrambled String](https://www.geeksforgeeks.org/check-if-a-string-is-a-scrambled-form-of-another-string/)\n",
    "\n",
    "Given two strings S1 and S2 of equal length, the task is to determine if S2 is a scrambled form of S1.\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {},
   "outputs": [],
   "source": [
    "A, B = 'rgtae', 'great'\n",
    "n = len(A)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Recursion"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 43,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def scrambled_string(A, B):\n",
    "    len_A = len(A)\n",
    "    len_B = len(B)\n",
    "    \n",
    "    if len_A != len_B:\n",
    "        return False\n",
    "    if len_A < 1 or len_B < 1:\n",
    "        return False\n",
    "    if A == B:\n",
    "        return True\n",
    "    if sorted(A) != sorted(B):\n",
    "        return False\n",
    "    \n",
    "    for i in range(1, len_B):\n",
    "        if scrambled_string(A[:i], B[:i]) and scrambled_string(A[i:], B[i:]):\n",
    "            return True\n",
    "        if scrambled_string(A[:i], B[-i:]) and scrambled_string(A[-i:], B[:i]):\n",
    "            return True\n",
    "    \n",
    "    return False\n",
    "\n",
    "scrambled_string(A, B)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "scrolled": true
   },
   "source": [
    "### Memomization"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "True"
      ]
     },
     "execution_count": 44,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(n)] for _ in range(n)]\n",
    "def scrambled_string_memo(A, B):\n",
    "    len_A = len(A)\n",
    "    len_B = len(B)\n",
    "    \n",
    "    if len_A != len_B:\n",
    "        return False\n",
    "    if len_A < 1 or len_B < 1:\n",
    "        return False\n",
    "    if A == B:\n",
    "        return True\n",
    "    if t[len_A-1][len_A-1] != -1:\n",
    "        return t[len_A-1][len_A-1]\n",
    "    if sorted(A) != sorted(B):\n",
    "        return False\n",
    "    \n",
    "    for i in range(1, len_B):\n",
    "        if scrambled_string_memo(A[:i], B[:i]) and scrambled_string_memo(A[i:], B[i:]):\n",
    "            t[i][i] = True\n",
    "            return True\n",
    "        if scrambled_string_memo(A[:i], B[-i:]) and scrambled_string_memo(A[-i:], B[:i]):\n",
    "            t[i][i] = True\n",
    "            return True\n",
    "    \n",
    "    t[i][i] = False\n",
    "    return False\n",
    "\n",
    "scrambled_string_memo(A, B)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## [Egg Droping Problem](https://www.geeksforgeeks.org/egg-dropping-puzzle-dp-11/)\n",
    "\n",
    "Given a certain amount of floors of a building (say f number of floors) and also given certain amount of eggs (say e number of eggs) …\n",
    "\n",
    "What is the least amount of egg drops one has to perform to find out the threshold floor? (Threshold floor is one from which the egg starts breaking and also egg breaks for all the floors above. Also, if egg dropped from any floor below the threshold floor, it won’t \n",
    "break.)\n",
    "\n",
    "Constraints:\n",
    "\n",
    "- An egg that survives a fall can be used again.\n",
    "\n",
    "- A broken egg must be discarded.\n",
    "\n",
    "- The effect of a fall is the same for all eggs.\n",
    "\n",
    "- If an egg breaks when dropped, then it would break if dropped from a higher floor.\n",
    "\n",
    "- If an egg survives a fall then it would survive a shorter fall.\n",
    "\n",
    "### Inputs"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [],
   "source": [
    "e = 3\n",
    "f = 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Recursion"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3"
      ]
     },
     "execution_count": 46,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import math\n",
    "\n",
    "def egg_droping(e, f):\n",
    "    if e == 0 or f == 0:\n",
    "        return 0\n",
    "    \n",
    "    if e == 1 or f == 1:\n",
    "        return f\n",
    "    \n",
    "    mn = math.inf\n",
    "    \n",
    "    for k in range(1, f+1):\n",
    "        temp = 1 + max(egg_droping(e-1, k-1), egg_droping(e, f-k))\n",
    "        mn = min(mn, temp)\n",
    "        \n",
    "    return mn\n",
    "\n",
    "egg_droping(e, f)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Memomization"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "3"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "t = [[-1 for _ in range(e)] for _ in range(f)]\n",
    "def egg_droping_memo(e, f):\n",
    "    if e == 0 or f == 0:\n",
    "        return 0\n",
    "    if e == 1 or f == 1:\n",
    "        return f\n",
    "    \n",
    "    if t[f-1][e-1] != -1:\n",
    "        return t[f-1][e-1]\n",
    "    \n",
    "    mn = math.inf\n",
    "    \n",
    "    for k in range(1, f+1):\n",
    "        temp = 1 + max(egg_droping_memo(e-1, k-1), egg_droping_memo(e, f-k))\n",
    "        mn = min(temp, mn)\n",
    "        \n",
    "    t[f-1][e-1] = mn\n",
    "    return mn\n",
    "\n",
    "egg_droping_memo(e, f)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# GeeksForGeeks DP"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### [Gold Mine Problem](https://www.geeksforgeeks.org/gold-mine-problem/)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "12"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "def gold_mine_problem(mat):\n",
    "    n = len(mat)\n",
    "    m = len(mat[0])\n",
    "    \n",
    "    cache = [[-1 for _ in range(m)] for _ in range(n)]\n",
    "    \n",
    "    def helper(i, j):\n",
    "        if cache[i][j] != -1:\n",
    "            return cache[i][j]\n",
    "        \n",
    "        if j == m-1:\n",
    "            cache[i][j] = mat[i][j]\n",
    "        else:\n",
    "            right_up = 0\n",
    "            if i != 0:\n",
    "                right_up = helper(i-1, j+1)\n",
    "            \n",
    "            right = helper(i, j+1)\n",
    "            \n",
    "            right_down = 0\n",
    "            if i != n-1:\n",
    "                right_down = helper(i+1, j+1)\n",
    "                \n",
    "            cache[i][j] = mat[i][j] + max(right_up, right, right_down)\n",
    "        \n",
    "        return cache[i][j]\n",
    "    \n",
    "    result = 0\n",
    "    for i in range(n):\n",
    "        result = max(result, helper(i, 0))\n",
    "        \n",
    "    return result\n",
    "\n",
    "gold_mine_problem([[1, 3, 3], [2, 1, 4], [0, 6, 4]])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Others\n",
    "\n",
    "### Length of Longest Bitonic Subsequence"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "def longestIncreasingSubsequence(A):\n",
    "    n = len(A)\n",
    "    dp = [0] * n\n",
    "\n",
    "    for i in range(n):\n",
    "        max_inc = 0\n",
    "        for j in range(i):\n",
    "            if A[j] < A[i]:\n",
    "                max_inc = max(max_inc, dp[j])\n",
    "        dp[i] = max_inc+1\n",
    "\n",
    "    return dp\n",
    "\n",
    "def longestDecreasingSubsequence(A):\n",
    "    n = len(A)\n",
    "    dp = [0] * n\n",
    "\n",
    "    for i in range(n-1, -1, -1):\n",
    "        max_dec = 0\n",
    "        for j in range(n-1, i, -1):\n",
    "            if A[j] < A[i]:\n",
    "                max_dec = max(max_dec, dp[j])\n",
    "        dp[i] = max_dec + 1\n",
    "\n",
    "    return dp \n",
    "\n",
    "def longestSubsequenceLength(A):\n",
    "    lis = longestIncreasingSubsequence(A)\n",
    "    lds = longestDecreasingSubsequence(A)\n",
    "    result = 0\n",
    "    for ele1, ele2 in zip(lis, lds):\n",
    "        result = max(result, ele1 + ele2 - 1)\n",
    "\n",
    "    return result"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.10"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
