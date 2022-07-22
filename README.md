# 로또 번호 생성기

## 초기 화면

## 주요 코드
```kotlin
로또 번호 생성 버튼

binding.btnNew.setOnClickListener {
            if (binding.cbIsSame.isChecked) { // 중복 허용이 체크 되었을 경우
                getLottoNumber1()
            } else { // 중복 허용이 체크 안되어있을 경우
                getLottoNumber2()
            }
            binding.btnSave.isEnabled = true
        }
```

```kotlin
저장하기 버튼

binding.btnSave.setOnClickListener { // 저장 버튼 클릭 시
            val lotto = IntArray(6)
            lotto[0] = binding.tvLotto1.text.toString().toInt()
            lotto[1] = binding.tvLotto2.text.toString().toInt()
            lotto[2] = binding.tvLotto3.text.toString().toInt()
            lotto[3] = binding.tvLotto4.text.toString().toInt()
            lotto[4] = binding.tvLotto5.text.toString().toInt()
            lotto[5] = binding.tvLotto6.text.toString().toInt()

            var strLotto = ""
            for (i in lotto.indices) {
                strLotto += lotto[i].toString()
                if (i != lotto.size - 1) strLotto += ","
            }
            if (strLotto.equals(lastSaved)) { // 방금 저장한 번호 일 경우
                Toast.makeText(this, "이미 저장했음", Toast.LENGTH_SHORT).show()
                return@setOnClickListener
            }
            idx++
            if (idx == 30) { // 저장공간이 꽉 찼을 경우
                Toast.makeText(this, "저장공간이 없음", Toast.LENGTH_SHORT).show()
                idx--
                return@setOnClickListener
            }
            listLotto[idx] = strLotto
            lastSaved = strLotto
            Toast.makeText(this, "저장완료!!", Toast.LENGTH_SHORT).show()
            Log.d("디버깅", strLotto)
            binding.btnShowList.isEnabled = true

            savePref()
        }
```
```kotlin
생성된 리스트 보기 버튼

binding.btnShowList.setOnClickListener {
            var strListLotto: String = ""
            for (i in 0..idx) {
                strListLotto += listLotto[i]
                if (i != idx) strListLotto += ","
            }

            val intent = Intent(this, ResultActivity::class.java)
            intent.putExtra("strListLotto", strListLotto)
            startActivity(intent)
        }
```
```kotlin
생성된 숫자 출력

private fun setTvNumber(i: Int, tv: TextView) {
        tv.text = i.toString()
        when (i) {
            in 1..10 -> tv.background = ContextCompat.getDrawable(this, R.drawable.circle_red)
            in 11..20 -> tv.background = ContextCompat.getDrawable(this, R.drawable.circle_orange)
            in 21..30 -> tv.background = ContextCompat.getDrawable(this, R.drawable.circle_pink)
            in 31..40 -> tv.background = ContextCompat.getDrawable(this, R.drawable.circle_mint)
            else -> tv.background = ContextCompat.getDrawable(this, R.drawable.circle_yellowgreen)
        }
    }
```

```kotlin
숫자 중복 허용함

private fun getLottoNumber1() {
        val rnd = Random()
        val lotto = IntArray(6)
        for (i in lotto.indices) { // indices 0..5
            lotto[i] = rnd.nextInt(45) + 1
        }
        lotto.sort() // 생성된 숫자 정렬
        setTvNumber(lotto[0], binding.tvLotto1) // 정렬한 순서대로 출력
        setTvNumber(lotto[1], binding.tvLotto2)
        setTvNumber(lotto[2], binding.tvLotto3)
        setTvNumber(lotto[3], binding.tvLotto4)
        setTvNumber(lotto[4], binding.tvLotto5)
        setTvNumber(lotto[5], binding.tvLotto6)
    }
```

```kotlin
숫자 중복 허용하지 않음

private fun getLottoNumber2() {
        val rnd = Random()
        val lotto = IntArray(6)
        while (true) {
            var isSame = false
            for (i in lotto.indices) {
                lotto[i] = rnd.nextInt(45) + 1
            }
            for (i in lotto.indices) {
                for (j in lotto.indices) {
                    if (i != j) { 
                        if (lotto[i] == lotto[j]) { // 숫자가 중복 될 경우
                            isSame = true 
                        }
                    }
                }
            }
            if (!isSame) { // 숫자가 중복이 없을 경우 빠져나옴
                break
            }
        }
        lotto.sort() // 생성된 숫자 정렬
        setTvNumber(lotto[0], binding.tvLotto1) // 정렬된 순서대로 출력
        setTvNumber(lotto[1], binding.tvLotto2)
        setTvNumber(lotto[2], binding.tvLotto3)
        setTvNumber(lotto[3], binding.tvLotto4)
        setTvNumber(lotto[4], binding.tvLotto5)
        setTvNumber(lotto[5], binding.tvLotto6)
    }
```
## 중복체크 되어있을 경우 
실행 화면
## 중복체크 안되어있을 경우
실행 화면

## 로또 번호 리스트 보기
실행 화면
## 로또 번호 초기화
실행 화면
