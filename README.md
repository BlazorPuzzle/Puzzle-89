# Puzzle #89 - Lighten the Load

Carl and Jeff want to know how to make a type-ahead search mechanism more efficient.

YouTube Video: https://youtu.be/w7gxuv2PywA

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with Global Server Interactivity.

We have an input box bound to a search string. On every keystroke it filters a list by the search term, which can be very chatty, especially after the first character which may return hundreds or even thousands of records.

How can we make this mechanism more efficient to lighten the load on the back end?

