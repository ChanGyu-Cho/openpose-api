https://hub.docker.com/r/bocker060/openpose-api

# OpenPose HTTP API (bocker060/openpose-api)

GPU-enabled Docker image exposing an HTTP API for 2D keypoint extraction (COCO-17 compatible).

---

## 🚀 Quickstart

```bash
docker run -d --gpus all -p 19030:19030 --name openpose-api bocker060/openpose-api
# Optional: ngrok
ngrok config add-authtoken <YOUR_TOKEN>
ngrok http 19030
```

# Endpoint

POST /openpose_predict
Base URL (default): http://localhost:19030/openpose_predict

Request (JSON)
Field	Type	Required	Description
img	string	✅	Base64-encoded image (PNG/JPG)
turbo_without_skeleton	boolean		If true, faster but some builds may omit immediate keypoints
Response (JSON example)
{
  "people": [
    { "pose_keypoints_2d": [x1, y1, c1, x2, y2, c2, ...] }
  ]
}


Some wrappers return 18 points (incl. Neck) or 17 (COCO).

Client can normalize to COCO-17.

# 🧪 Batch client → CSV (COCO-17)

Use the provided client to process all images and write skeleton2d.csv:

```bash
python openpose-api_test.py --image-dir ./color --output ./result
```

## → ./result/skeleton2d.csv
## Columns: Nose_x, Nose_y, Nose_c, LEye_x, ..., RAnkle_c

# 🧱 Pipeline (context)
[Images/Frames] --POST--> OpenPose API --CSV--> skeleton2d.csv

🛠️ Troubleshooting

GPU visibility: NVIDIA drivers + NVIDIA Container Toolkit; test with --gpus all.

Connection/timeout: docker logs openpose-api, port mapping, firewall.

No detections: lighting/framing; inspect result/raw_responses/*.json.

BODY_25 vs COCO: verify model; client remaps to COCO-17 when possible.

# 🇰🇷 한글 요약

실행: docker run -d --gpus all -p 19030:19030 --name openpose-api bocker060/openpose-api

엔드포인트: POST /openpose_predict (이미지 base64 입력 → 키포인트 JSON)

배치 변환: openpose-api_test.py로 skeleton2d.csv 생성(COCO-17)
