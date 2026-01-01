# CodeWars-Moving-Zeros-To-The-End-5-kyu---Passed-
Write an algorithm that takes an array and moves all of the zeros to the end, preserving the order of the other elements.

moveZeros([false,1,0,1,2,0,1,3,"a"]) // returns[false,1,1,2,1,3,"a",0,0]


#[cfg(test)]
mod tests {
    use rand::{thread_rng, Rng};
    use rand::seq::SliceRandom;
    use super::move_zeros;
    use std::iter;
    use itertools::Itertools;

    fn reference_solution(arr: &[u8]) -> Vec<u8> {
        arr.iter().sorted_by_key(|x| x == &&0).map(|x| *x).collect()
    }
    
    fn dotest(a: &[u8], expected: &[u8]) {
        let actual = move_zeros(a);
        assert!(actual == expected, "With arr = {a:?}\nExpected {expected:?} but got {actual:?}")   
    }

    #[test]
    fn sample_tests() {
        dotest(&[1, 2, 0, 1, 0, 1, 0, 3, 0, 1], &[1, 2, 1, 1, 3, 1, 0, 0, 0, 0]);
        dotest(&[9, 0, 0, 9, 1, 2, 0, 1, 0, 1, 0, 3, 0, 1, 9, 0, 0, 0, 0, 9], &[9, 9, 1, 2, 1, 1, 3, 1, 9, 9, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]);
        dotest(&[0, 0], &[0, 0]);
        dotest(&[0], &[0]);
        dotest(&[], &[]);
    }
    
    #[test]
    fn random_tests() {
        let mut rng = thread_rng();
        let mut cases = vec![vec![], vec![0], vec![1], vec![2], vec![0, 0], vec![0, 1], vec![0, 2], vec![1, 0], vec![1, 1], vec![1, 2], vec![2, 0], vec![2, 1], vec![2, 2], vec![0, 0, 0], vec![0, 0, 1], vec![0, 0, 2], vec![0, 1, 0], vec![0, 1, 1], vec![0, 1, 2], vec![0, 2, 0], vec![0, 2, 1], vec![0, 2, 2], vec![1, 0, 0], vec![1, 0, 1], vec![1, 0, 2], vec![1, 1, 0], vec![1, 1, 1], vec![1, 1, 2], vec![1, 2, 0], vec![1, 2, 1], vec![1, 2, 2], vec![2, 0, 0], vec![2, 0, 1], vec![2, 0, 2], vec![2, 1, 0], vec![2, 1, 1], vec![2, 1, 2], vec![2, 2, 0], vec![2, 2, 1], vec![2, 2, 2], vec![0, 0, 0, 0], vec![0, 0, 0, 1], vec![0, 0, 0, 2], vec![0, 0, 1, 0], vec![0, 0, 1, 1], vec![0, 0, 1, 2], vec![0, 0, 2, 0], vec![0, 0, 2, 1], vec![0, 0, 2, 2], vec![0, 1, 0, 0], vec![0, 1, 0, 1], vec![0, 1, 0, 2], vec![0, 1, 1, 0], vec![0, 1, 1, 1], vec![0, 1, 1, 2], vec![0, 1, 2, 0], vec![0, 1, 2, 1], vec![0, 1, 2, 2], vec![0, 2, 0, 0], vec![0, 2, 0, 1], vec![0, 2, 0, 2], vec![0, 2, 1, 0], vec![0, 2, 1, 1], vec![0, 2, 1, 2], vec![0, 2, 2, 0], vec![0, 2, 2, 1], vec![0, 2, 2, 2], vec![1, 0, 0, 0], vec![1, 0, 0, 1], vec![1, 0, 0, 2], vec![1, 0, 1, 0], vec![1, 0, 1, 1], vec![1, 0, 1, 2], vec![1, 0, 2, 0], vec![1, 0, 2, 1], vec![1, 0, 2, 2], vec![1, 1, 0, 0], vec![1, 1, 0, 1], vec![1, 1, 0, 2], vec![1, 1, 1, 0], vec![1, 1, 1, 1], vec![1, 1, 1, 2], vec![1, 1, 2, 0], vec![1, 1, 2, 1], vec![1, 1, 2, 2], vec![1, 2, 0, 0], vec![1, 2, 0, 1], vec![1, 2, 0, 2], vec![1, 2, 1, 0], vec![1, 2, 1, 1], vec![1, 2, 1, 2], vec![1, 2, 2, 0], vec![1, 2, 2, 1], vec![1, 2, 2, 2], vec![2, 0, 0, 0], vec![2, 0, 0, 1], vec![2, 0, 0, 2], vec![2, 0, 1, 0], vec![2, 0, 1, 1], vec![2, 0, 1, 2], vec![2, 0, 2, 0], vec![2, 0, 2, 1], vec![2, 0, 2, 2], vec![2, 1, 0, 0], vec![2, 1, 0, 1], vec![2, 1, 0, 2], vec![2, 1, 1, 0], vec![2, 1, 1, 1], vec![2, 1, 1, 2], vec![2, 1, 2, 0], vec![2, 1, 2, 1], vec![2, 1, 2, 2], vec![2, 2, 0, 0], vec![2, 2, 0, 1], vec![2, 2, 0, 2], vec![2, 2, 1, 0], vec![2, 2, 1, 1], vec![2, 2, 1, 2], vec![2, 2, 2, 0], vec![2, 2, 2, 1], vec![2, 2, 2, 2]];
        for i in 0..10 {
            cases.extend([(0..i).map(|_| rng.gen_range(1..10)).collect(), (0..i).map(|_| rng.gen_range(1..10)).collect()]);
        }
        for (zero, nonzero) in itertools::iproduct!(5..10, 0..5) {
            let mut xs = (0..nonzero).map(|_| rng.gen_range(1..10)).chain(iter::repeat(0).take(zero)).collect::<Vec<_>>();
            xs.shuffle(&mut rng);
            cases.push(xs)
        }
        cases.shuffle(&mut rng);
        for arr in cases.iter() {
            dotest(arr, &reference_solution(arr));
        }
    }
}
