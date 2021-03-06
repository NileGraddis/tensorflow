#
# Copyright 2020 The TensorFlow Authors. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
# Need a separate function because of the .cc vs .cpp used in the one provided by MLIR
function(add_mlir_hlo_dialect dialect dialect_namespace)
  set(LLVM_TARGET_DEFINITIONS ${dialect}.td)
  mlir_tablegen(${dialect}.h.inc -gen-op-decls)
  mlir_tablegen(${dialect}.cc.inc -gen-op-defs)
  mlir_tablegen(${dialect}_structs.h.inc -gen-struct-attr-decls)
  mlir_tablegen(${dialect}_structs.cc.inc -gen-struct-attr-defs)
  add_public_tablegen_target(MLIR${dialect}IncGen)
  add_dependencies(mlir-headers MLIR${dialect}IncGen)
endfunction()

add_mlir_hlo_dialect(chlo_ops chlo)
add_mlir_hlo_dialect(lhlo_ops lmhlo)

set(LLVM_TARGET_DEFINITIONS hlo_ops.td)
mlir_tablegen(hlo_ops.h.inc -gen-op-decls)
mlir_tablegen(hlo_ops.cc.inc -gen-op-defs)
mlir_tablegen(hlo_ops_base_structs.h.inc -gen-struct-attr-decls)
mlir_tablegen(hlo_ops_base_structs.cc.inc -gen-struct-attr-defs)
mlir_tablegen(hlo_ops_base_enums.h.inc -gen-enum-decls)
mlir_tablegen(hlo_ops_base_enums.cc.inc -gen-enum-defs)
add_public_tablegen_target(MLIRhlo_opsIncGen)

set(LLVM_TARGET_DEFINITIONS lhlo_gpu_ops.td)
mlir_tablegen(lhlo_gpu_ops.h.inc -gen-op-decls)
mlir_tablegen(lhlo_gpu_ops.cc.inc -gen-op-defs)
set(LLVM_TARGET_DEFINITIONS lhlo_gpu_ops_structs.td)
mlir_tablegen(lhlo_gpu_ops_structs.h.inc -gen-struct-attr-decls)
mlir_tablegen(lhlo_gpu_ops_structs.cc.inc -gen-struct-attr-defs)
set(LLVM_TARGET_DEFINITIONS lhlo_gpu_ops_enums.td)
mlir_tablegen(lhlo_gpu_ops_enums.h.inc -gen-enum-decls)
mlir_tablegen(lhlo_gpu_ops_enums.cc.inc -gen-enum-defs)
add_public_tablegen_target(MLIRlhlo_gpu_opsIncGen)
add_dependencies(mlir-headers MLIRlhlo_gpu_opsIncGen)

add_mlir_interface(infer_fusibility_op_interface)
