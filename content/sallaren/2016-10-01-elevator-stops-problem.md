---
utid: 20161001213800
date: 2016-10-01 21:38:00
title: Elevator Stops Problem + Solution
_index: elevator-stops-problem
tags:
  - Programming
---
Recently I went to a technical interview in a big tech company for the front-end engineering role. Regardless of how I feel about their process and how unrelated the test was to the job, I found one of the questions interesting and thought it would be nice if I post my solution here.

Here’s the problem:

> There is an elevator in a building with `M` floors. This elevator can take a max of `X` people at a time or max of total weight `Y`. Given that a set of people and their weight and the floor they need to stop, how many stops has the elevator taken to serve all the people? Consider the elevator serves in "first come first serve" basis and the order for the queue can not be changed.

> **Example:**

> Let Array `A` be the weight of each person `A = [60, 80, 40]`.
> Let Array `B` be the floors where each person needs to be dropped off `B = [2, 3, 5]`.

> The building has 5 floors, maximum of 2 persons are allowed in the elevator at a time with max weight capacity being 200. For this example, the elevator would take total of 5 stops in floors: **2**, **3**, **G**, **5** and finally **G**.

And here’s my solution:

    /**
     * Remove duplicate items from an array
     * @param {Array} arr
     * @returns {Array}
     */
    function uniq(arr) {
        return arr.reduce((prev, curr) => {
            if (prev.indexOf(curr) === -1) {
                prev = prev.concat(curr);
            }
            return prev;
        }, []);
    }

    /**
     * Solution to our problem
     *
     * @param {Array.<Number>}  A Array of passengers weights
     * @param {Array.<Number>}  B Array of passenger destination floors
     * @param {Number}          M Number of floors in the building
     * @param {Number}          X Elevator max passenger capacity
     * @param {Number}          Y Elevator max weight capacity
     * @returns {Number}        Number of total stops
     */
    function solution(A, B, M, X, Y) {
        let trip = 0,
            tripWeight = 0,
            rounds = [];

        for (let i = 0, len = A.length; i < len; i += 1) {
            // If there's an unclosed trip, let’s see if we can get more people in
            if (typeof rounds[trip] !== 'undefined') {
                // Check if we have filled the capacity for the current trip,
                // if so, then close the existing trip and create a new one
                if (rounds[trip].length === X || tripWeight + A[i] > Y) {
                    // Increase trip count
                    trip++;
                    // Reset current weight
                    tripWeight = 0;
                }
            }

            // Create an empty array for the current trip
            rounds[trip] = rounds[trip] || [];
            // Push passengers destination to current trip
            rounds[trip].push(B[i]);
            // Increase current load
            tripWeight += A[i];
        }

        // Remove duplicate floors from each trip, since
        // the elevator will make 1 stop for pessengers that
        // go to the same floor. Then add 1 (return to ground floor)
        rounds = rounds.map(round => uniq(round).length + 1);

        // To get number of total stops, we sum up
        // destination count in each trip.
        return rounds.reduce((prev, curr) => prev + curr, 0);
    }

    console.log(
        solution(
            [60, 80, 40],   // Weights
            [2, 3, 5],      // Destinations
            5,              // Floors
            2,              // Max passengers
            200             // Max weight
        )
    ); // -> 5


After I successfully submitted the solution, I searched Google to see if this question has been asked before, turns out it’s a common problem that is asked in programming tests and many people have solved it ([for example this one](http://codereview.stackexchange.com/questions/111673/program-to-determine-total-stops-taken-by-elevator)).

I hope this solution comes in handy for anybody who's willing to cheat a programming test. Hope you don’t though :-)
