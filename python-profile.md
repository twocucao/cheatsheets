---
title: Python Profile
---

## 性能测试

### timeit

    标准库自带，测量少量代码片段的执行时间
    1. 每块代码执行多次，以便有足够长的统计时间。
    2. 上一步执行多次，计算单次平均值
    3. 统计最短采样，计算单次平均值

    python -m timeit -n 10 -r 3 -s "import time" "time.sleep(1)"

    在 jupyter 中还可以这么搞
    %time time.sleep(1)
    %timeit -n 10 -r 3 time.sleep(1)

### profile

    python -m cProfile -s cumtime test.py
    # ncall 被调用次数
    # totime 总执行时间，不包含其调用的子函数
    # cumtime 执行总耗时，包括其调用的子函数

### line profiler

    kernprof -l -v test.py
    %load_ext line_profiler
    %lprun -f run -f test run()

### memory profiler

    python -m memory_profiler test.py
    %load_ext memory_profiler
    %mprun -f test test()
    %memit test()

    # 还可以对添加了 profile 装饰器的函数做重点标记。从而输出 matplotlib

### pympler

    # STEP1. 检查整体情况
    from pympler import summary, muppy
    all_objects = muppy.get_objects()
    s = summary.sumarize(all_objects)
    summary.print_(s)

    # STEP2. 在执行点跟踪快照，在不同的地方追踪快照，从而缩小问题范围
    from pympler.tracker import SummaryTracker
    tr = SummaryTracker()

    # STEP3. 定位具体类型
    from pympler.tracker import ClassTracker
    tr = ClassTracker()

    # STEP4. 递归统计个体
    from pympler.asizeof import asizeof # 比 sys.get_size_of

    # STEP5. 查看详情
    from pympler.asizeof import asized
    print(asized(obj, detail = 1).format())

### pdb 与 ipdb

    调试


