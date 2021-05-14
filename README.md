# shared_memory
a simple thread safe shared_memory concept

## Usage
```cpp
#include <thread>
#include "shared_memory.hpp"


int main()
{
    using sm_dq_int = stdx::shared_memory<int>; // deque<int> shared memory
    using sm_vec_int = stdx::shared_memory<int, std::vector>; // vector<int> shared memory

    std::thread t1([&]{
        for(int i=0; i<10000; ++i){
            sm_vec_int::push_back(i);
        }});

    std::thread t2([&]{
        for(int i=10000; i<20000; ++i){
            sm_vec_int::push_back(i);
        }});

    std::thread t4([&]{
        for(auto &i : sm_vec_int::data()){
            std::cout<<i<<" ";
        }});

    t1.join();
    t2.join();
    t4.join();

}
```
