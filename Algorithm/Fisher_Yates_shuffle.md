https://zhuanlan.zhihu.com/p/110630952

https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=562160&highlight=snowflake


```C++
#include <cstdint>
#include <iostream>
#include <limits>
#include <random>
#include <unordered_map>
#include <unordered_set>
using uint32 = std::uint32_t;
// 生成[0, 1]区间内的随机小数
class NumericUniformSamplingWithReplacement {
public:
  NumericUniformSamplingWithReplacement() : mt_(rd_()), dist_(0, 1) {}
  double NextValue() { return dist_(mt_); }
private:
  std::random_device rd_;
  std::mt19937 mt_;
  std::uniform_real_distribution<double> dist_;
};
// 生成[0, 2^32-1]区间内的整数
class IntegerUniformSamplingWithoutReplacement {
public:
  uint32 NextValue() {
    const uint32 sampling_range =
        std::numeric_limits<uint32>::max() - current_index_;
    const uint32 exchange_index =
        current_index_ +
        static_cast<uint32>(sampling_range * uniform_zero_one_.NextValue());
    const uint32 current_value = GetValue(current_index_);
    const uint32 exchange_value = GetValue(exchange_index);
    // 每次执行 NextValue()，至多向 values_ 中存入一个新值
    values_[exchange_index] = current_value;
    ++current_index_;
    return exchange_value;
  }
  size_t GetNumStoredValues() const { return values_.size(); }
private:
  uint32 GetValue(uint32 index) {
    const auto it = values_.find(index);
    return it == values_.end() ? index : it->second;
  }
  NumericUniformSamplingWithReplacement uniform_zero_one_;
  uint32 current_index_ = 0;
  std::unordered_map<uint32, uint32> values_;
};
int main(int argc, char* argv[]) {
  IntegerUniformSamplingWithoutReplacement random;
  // 生成1000000个数，验证不重复
  constexpr uint32 kValidationSize = 1000000;
  std::unordered_set<uint32> visited;
  visited.reserve(kValidationSize);
  for (uint32 i = 0; i < kValidationSize; ++i) {
    const uint32 value = random.NextValue();
    if (!visited.emplace(value).second) {
      std::cerr << "Error: duplicated " << value << "\n";
    }
  }
  // 这里显示的存储的值数量不会超过 kValidationSize
  std::cout << "# store values = " << random.GetNumStoredValues() << "\n";
  return 0;
}

```