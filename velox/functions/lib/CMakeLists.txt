# Copyright (c) Facebook, Inc. and its affiliates.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
find_library(RE2 re2 REQUIRED)

add_library(velox_is_null_functions IsNull.cpp)

target_link_libraries(velox_is_null_functions velox_expression)

add_library(velox_functions_util LambdaFunctionUtil.cpp RowsTranslationUtil.cpp)

target_link_libraries(velox_functions_util velox_vector velox_common_base)

add_library(
  velox_functions_lib
  CheckDuplicateKeys.cpp
  DateTimeFormatter.cpp
  DateTimeFormatterBuilder.cpp
  KllSketch.cpp
  LambdaFunctionUtil.cpp
  MapConcat.cpp
  Re2Functions.cpp
  StringEncodingUtils.cpp)

target_link_libraries(velox_functions_lib velox_vector ${RE2}
                      ${FOLLY_WITH_DEPENDENCIES})

add_subdirectory(string)
if(${VELOX_BUILD_TESTING})
  add_subdirectory(tests)
endif()

if(${VELOX_ENABLE_BENCHMARKS})
  add_subdirectory(benchmarks)
endif()
