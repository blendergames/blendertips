
# 블렌더에서 다른 UV 기준으로 텍스처 베이크하기

## 1. UV 맵 준비
1. 오브젝트를 선택하고 **UV Editing** 탭으로 이동.
2. **UV Map**을 두 개 이상 만듭니다.  
   - `UVMap_old` → 기존 텍스처가 맞춰져 있는 UV.
   - `UVMap_new` → 새로 적용하고 싶은 UV.
3. 새 UV에서 원하는 대로 언랩(unwrap)합니다.

---

## 2. 베이크용 머티리얼 설정
1. **Material Properties**에서 기존 텍스처가 연결된 머티리얼 확인.
2. 새 이미지 텍스처를 만듭니다. (`Image > New`)  
   - 해상도, 이름 지정 (`BakeTarget.png` 등).
   - 저장 (`Image > Save As`) 해둡니다.

---

## 3. 베이크 시 **출발 UV → 목표 UV** 지정

Blender의 기본 베이크는 **활성 UV 맵**을 기준으로 하기 때문에,  
원래 UV를 읽고, 새 UV에 쓸 수 있도록 설정이 필요합니다.

방법은 크게 두 가지가 있습니다.

### 방법 A — **Transfer 방식**
1. 두 개의 동일한 메쉬를 준비합니다.  
   - `Obj_Source` → 원래 텍스처가 맞춰진 UVMap_old만 활성.
   - `Obj_Target` → 새 UVMap_new만 활성, 베이크 받을 새 이미지 연결.
2. `Obj_Target`을 활성화하고 `Obj_Source`를 **Shift+클릭**하여 둘 다 선택.
3. **Render Properties > Bake**  
   - Bake Type: **Diffuse** (Color Only 체크)
   - **Selected to Active** 활성화
   - Ray Distance를 적당히 조절.
4. Bake 실행 → 새 UV 기준으로 텍스처 생성.

### 방법 B — **Same Object 내에서 UV 전환**
이건 노드에서 UV를 명시적으로 지정해주는 방식입니다.
1. Shader Editor에서 **UV Map 노드** 추가.
2. "원본 UV"와 "새 UV"를 각각 다른 UV Map 노드에 연결.
3. 원본 UV를 사용해 텍스처 샘플링, 새 UV에 연결된 이미지로 베이크.
4. Bake 실행 시, 활성 UV가 새 UV일 때 베이크하면 됩니다.

---

## 💡 팁
- 원본 텍스처 해상도보다 너무 낮게 하면 디테일이 깨집니다.
- Normal map이나 Roughness도 같은 방식으로 개별 베이크 가능.
- Cycles 렌더 엔진에서만 **Selected to Active**가 동작하니, Eevee는 안 됩니다.
