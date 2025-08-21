# 복지용구 RAG 챗봇 프로젝트

복지용구 관련 공지사항과 법령자료를 크롤링하고, 텍스트화하여 RAG(Retrieval-Augmented Generation) 기반 챗봇으로 제공하는 프로젝트입니다.

## 🏗️ 프로젝트 구조

```
webcr/
├── 📱 챗봇 앱
│   ├── app.py          # 기본 RAG 챗봇
│   ├── app1.py         # 성능 최적화 버전
│   ├── app2.py         # 하이브리드 검색 버전
│   ├── app3.py         # 섹션 인지 + 청크 번들링 버전
│   └── app챗봇.md      # 챗봇 앱 발전 과정 상세 설명
├── 🕷️ 크롤링 코드
│   ├── crawlers/
│   │   ├── req2.py     # 복지용구 공지사항 크롤러
│   │   ├── req3.py     # 복지용구 법령자료실 크롤러
│   │   └── ...
│   ├── req2크롤러.md   # 공지사항 크롤러 상세 설명
│   └── req3크롤러.md   # 법령자료실 크롤러 상세 설명
├── 📄 텍스트 추출
│   ├── good_all.py     # PDF/HWP 텍스트 추출 메인 스크립트
│   └── good_all텍스트추출.md  # 텍스트 추출 파이프라인 상세 설명
├── 📊 데이터 파일
│   ├── rag_input_sample.json      # 공지사항 텍스트화 결과
│   ├── rag_input_sample1.json    # 법령자료실 텍스트화 결과
│   └── noin3_data.json           # noin3.pdf 텍스트화 결과
└── 📁 기타
    ├── attachments/               # 공지사항 첨부파일
    ├── attachments1/              # 법령자료실 첨부파일
    └── vectorstore/               # FAISS 벡터 인덱스
```

## 🚀 주요 기능

### 1. **챗봇 시스템** (`app.py` → `app3.py`)
- **app.py**: 기본 RAG 챗봇 (FAISS + GPT-4)
- **app1.py**: 성능 최적화 (싱글톤, MMR 검색, 날짜 우선순위)
- **app2.py**: 하이브리드 검색 (FAISS + BM25 앙상블)
- **app3.py**: 섹션 인지 + 청크 번들링 (고급 컨텍스트 관리)

📖 **자세한 설명**: [app챗봇.md](app챗봇.md)

### 2. **크롤링 시스템**
- **req2.py**: 복지용구 공지사항 크롤링 (커뮤니티 키: B0022)
- **req3.py**: 복지용구 법령자료실 크롤링 (커뮤니티 키: B0018)

📖 **자세한 설명**: [req2크롤러.md](req2크롤러.md), [req3크롤러.md](req3크롤러.md)

### 3. **텍스트 추출 시스템**
- **good_all.py**: PDF/HWP/기타 문서 텍스트 추출 및 JSON 변환
- 지원 형식: PDF(텍스트+OCR), HWP, XLSX, XLS, ZIP

📖 **자세한 설명**: [good_all텍스트추출.md](good_all텍스트추출.md)

## 📋 데이터 흐름

```
1. 크롤링 (req2.py, req3.py)
   ↓
2. 첨부파일 다운로드 (PDF, HWP, XLSX 등)
   ↓
3. 텍스트 추출 (good_all.py)
   ↓
4. JSON 데이터 생성
   ↓
5. 벡터 인덱싱 (FAISS)
   ↓
6. RAG 챗봇 응답
```

## 📁 주요 데이터 파일

| 파일명 | 설명 | 크롤링 소스 |
|--------|------|-------------|
| `rag_input_sample.json` | 공지사항 텍스트화 결과 | req2.py (복지용구 공지사항) |
| `rag_input_sample1.json` | 법령자료실 텍스트화 결과 | req3.py (복지용구 법령자료실) |
| `noin3_data.json` | noin3.pdf 텍스트화 결과 | 수동 PDF 처리 |

## 🛠️ 기술 스택

- **백엔드**: Flask, Python
- **AI/ML**: OpenAI GPT-4, LangChain, FAISS
- **크롤링**: requests, BeautifulSoup
- **문서 처리**: PyMuPDF, olefile, pytesseract
- **검색**: BM25, MMR, Ensemble Retrieval
- **데이터**: JSON, CSV, FAISS 벡터 인덱스

## 🚀 시작하기

### 1. 환경 설정
```bash
pip install -r requirements.txt
```

### 2. 크롤링 실행
```bash
# 공지사항 크롤링
python crawlers/req2.py

# 법령자료실 크롤링
python crawlers/req3.py
```

### 3. 텍스트 추출
```bash
python good_all.py
```

### 4. 챗봇 실행
```bash
# 기본 버전
python app.py

# 최적화 버전
python app1.py

# 하이브리드 검색 버전
python app2.py

# 섹션 인지 버전
python app3.py
```

## 📚 문서

- [챗봇 앱 발전 과정](app챗봇.md)
- [공지사항 크롤러 설명](req2크롤러.md)
- [법령자료실 크롤러 설명](req3크롤러.md)
- [텍스트 추출 파이프라인](good_all텍스트추출.md)

## 🔧 설정

- `.env` 파일에 OpenAI API 키 설정 필요
- Tesseract OCR 설치 필요 (한글 인식용)
- 한컴오피스 설치 필요 (HWP 변환용)

## 📝 라이선스

이 프로젝트는 교육 및 연구 목적으로 제작되었습니다.

## 🤝 기여

프로젝트 개선을 위한 제안이나 버그 리포트는 언제든 환영합니다!
