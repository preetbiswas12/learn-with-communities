#For video reference see this video [Youtube Link](https://youtu.be/VvFVkWseY08?si=KI_B0OVaBJz6h2hi)


The codes used in this lab are given below, simply copy and paste them in the Cloud Shell terminal to complete the lab and see in the video that on when part the codes are used.

```bash
cat > lab7.sh <<'LABEOF'
#!/bin/bash
# Universal solver (v2): Store, Process, and Manage Data on Google Cloud - Challenge Lab (ARC100)

ask() { local r; read -e -r -p "$2 [$3]: " r; printf -v "$1" '%s' "${r:-$3}"; }
die() { echo; echo "STOP: $1"; exit 1; }

IMG_URL="https://storage.googleapis.com/cloud-training/arc101/travel.jpg"

PROJECT_ID=$(gcloud config get-value project 2>/dev/null)
if [ -z "$PROJECT_ID" ] || [ "$PROJECT_ID" = "(unset)" ]; then
  die "no project set."
fi
PROJECT_NUMBER=$(gcloud projects describe "$PROJECT_ID" --format="value(projectNumber)")

DET_REGION=$(gcloud config get-value compute/region 2>/dev/null)
if [ -z "$DET_REGION" ] || [ "$DET_REGION" = "(unset)" ]; then
  DET_REGION="us-east4"
fi

echo "=== Enter values ==="
ask REGION   "Region"                                                 "$DET_REGION"
ask ZONE     "Zone"                                                   "${REGION}-c"
ask BUCKET   "Bucket name"                                            "memories-bucket-${PROJECT_ID}"
ask TOPIC    "Pub/Sub topic name"                                     "memories-topic-419"
ask FUNCTION "Cloud Run function name"                                "memories-thumbnail-generator"
ask RUNTIME  "Runtime"                                                 "nodejs20"

gcloud config set compute/region "$REGION" >/dev/null 2>&1
gcloud config set compute/zone "$ZONE" >/dev/null 2>&1

gcloud services enable cloudfunctions.googleapis.com run.googleapis.com eventarc.googleapis.com \
  pubsub.googleapis.com cloudbuild.googleapis.com artifactregistry.googleapis.com \
  storage.googleapis.com logging.googleapis.com

if gcloud storage buckets describe "gs://$BUCKET" >/dev/null 2>&1; then
  echo "bucket already exists"
else
  gcloud storage buckets create "gs://$BUCKET" --location="$REGION"
fi

gcloud pubsub topics describe "$TOPIC" >/dev/null 2>&1 || gcloud pubsub topics create "$TOPIC"

gcloud services identity create --service=pubsub.googleapis.com --project="$PROJECT_ID" >/dev/null 2>&1
gcloud services identity create --service=eventarc.googleapis.com --project="$PROJECT_ID" >/dev/null 2>&1

GCS_SA=$(gcloud storage service-agent --project="$PROJECT_ID" 2>/dev/null)
COMPUTE_SA="${PROJECT_NUMBER}-compute@developer.gserviceaccount.com"
PUBSUB_SA="service-${PROJECT_NUMBER}@gcp-sa-pubsub.iam.gserviceaccount.com"
EVENTARC_SA="service-${PROJECT_NUMBER}@gcp-sa-eventarc.iam.gserviceaccount.com"

grant() {
  gcloud projects add-iam-policy-binding "$PROJECT_ID" --member="$1" --role="$2" --condition=None --quiet 2>/dev/null
}

[ -n "$GCS_SA" ] && grant "serviceAccount:$GCS_SA" roles/pubsub.publisher
grant "serviceAccount:$COMPUTE_SA" roles/eventarc.eventReceiver
grant "serviceAccount:$COMPUTE_SA" roles/run.invoker
grant "serviceAccount:$COMPUTE_SA" roles/pubsub.publisher
grant "serviceAccount:$COMPUTE_SA" roles/storage.objectAdmin
grant "serviceAccount:$COMPUTE_SA" roles/artifactregistry.writer
grant "serviceAccount:$COMPUTE_SA" roles/logging.logWriter
grant "serviceAccount:$COMPUTE_SA" roles/cloudbuild.builds.builder
grant "serviceAccount:$PUBSUB_SA" roles/iam.serviceAccountTokenCreator
grant "serviceAccount:$EVENTARC_SA" roles/eventarc.serviceAgent

mkdir -p ~/thumbnail-function && cd ~/thumbnail-function
cat > index.js <<'JS1'
const functions = require('@google-cloud/functions-framework');
const { Storage } = require('@google-cloud/storage');
const { PubSub } = require('@google-cloud/pubsub');
const sharp = require('sharp');

functions.cloudEvent('FUNCTION_NAME_PLACEHOLDER', async cloudEvent => {
  const event = cloudEvent.data;
  const fileName = event.name;
  const bucketName = event.bucket;
  const bucket = new Storage().bucket(bucketName);
  const topicName = "TOPIC_NAME_PLACEHOLDER";
  const pubsub = new PubSub();

  if (fileName.search("64x64_thumbnail") === -1) {
    const filename_split = fileName.split('.');
    const filename_ext = filename_split[filename_split.length - 1].toLowerCase();
    const filename_without_ext = fileName.substring(0, fileName.length - filename_ext.length - 1);

    if (filename_ext === 'png' || filename_ext === 'jpg' || filename_ext === 'jpeg') {
      const gcsObject = bucket.file(fileName);
      const newFilename = `${filename_without_ext}_64x64_thumbnail.${filename_ext}`;
      const gcsNewObject = bucket.file(newFilename);

      try {
        const [buffer] = await gcsObject.download();
        const resizedBuffer = await sharp(buffer).resize(64, 64, { fit: 'inside', withoutEnlargement: true }).toFormat(filename_ext).toBuffer();
        await gcsNewObject.save(resizedBuffer, { metadata: { contentType: `image/${filename_ext}` } });
        await pubsub.topic(topicName).publishMessage({ data: Buffer.from(newFilename) });
      } catch (err) { console.error(`Error: ${err}`); }
    }
  }
});
JS1

cat > package.json <<'JSON1'
{
 "name": "thumbnails",
 "version": "1.0.0",
 "main": "index.js",
 "dependencies": {
   "@google-cloud/functions-framework": "^3.0.0",
   "@google-cloud/pubsub": "^2.0.0",
   "@google-cloud/storage": "^6.11.0",
   "sharp": "^0.32.1"
 }
}
JSON1

sed -i "s/TOPIC_NAME_PLACEHOLDER/${TOPIC}/; s/FUNCTION_NAME_PLACEHOLDER/${FUNCTION}/" index.js

gcloud functions deploy $FUNCTION \
  --gen2 \
  --runtime $RUNTIME \
  --entry-point $FUNCTION \
  --source . \
  --region $REGION \
  --trigger-bucket $BUCKET \
  --trigger-location $REGION \
  --max-instances 1 \
  --quiet

wget -qO travel.jpg "$IMG_URL"
gcloud storage cp travel.jpg "gs://$BUCKET/travel.jpg"
echo "Deployment Complete!"
LABEOF

```

then 

```bash
bash lab7.sh
```
Click `Check my progress` on every task to see the result.