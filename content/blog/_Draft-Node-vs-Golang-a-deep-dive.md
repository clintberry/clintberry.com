---
title: Node vs Golang, A Deep Dive
draft: true
author: Clint Berry
layout: post
date: 2015-09-01
url: /2015/node-vs-golang-a-deep-dive
categories:
  - NodeJS
  - Golang
---

# Introduction
 * I love NodeJS
   * Deployed in production v0.2
   * But I also admit to its failings... https://www.destroyallsoftware.com/talks/wat
   * I don't know most of the features of ES6

# Language Design Goals and Commonalities (Why Compare the Two at all?)
 * Concurrency is built in, in contrast to Python (need: Twisted, Tornado) and Ruby (elixer)

# Types: the good, the bad, the really ugly...

# Syntax Differences
 * White Space significance
 * 

# Scoping
 * Callback scope in JS
 * Go Routine scope in Go



# Basic Concurrency Patterns
 * Callback vs Go-routine
 * Async Web Server


# Garbage Collection Methods
 * Techniques used by both systems
 * Node GC: https://strongloop.com/strongblog/node-js-performance-garbage-collection/
 * Go GC: https://talks.golang.org/2015/go-gc.pdf
 * Websocket example
