1. 完成了rustlings全部quiz
2. 尝试了5道tolnay的rust-quiz
3. 刷新了rust的macros的知识， 参考并理解了一下资料
		1. https://danielkeep.github.io/tlborm/book/mbe-macro-rules.html
		2. https://doc.rust-lang.org/reference/macros-by-example.html
4. 了解了HRTB(Higher Rank Trait Bounds)
5. 阅读了OS课程slides的前三讲

-----------
坑:		
1. macro解析不会lookahead；https://doc.rust-lang.org/reference/macros-by-example.html#transcribing
2. 上一级macro调用下一级macro，不会再match literal，除非是tt,lifetime,ident，参考同上
