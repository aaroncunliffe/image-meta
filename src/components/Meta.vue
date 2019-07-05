<template>
  <b-container>
    <b-row>
      <b-col>
        <div class="pt-4 pb-2">
          <p>
            Upload your image to see all of the metadata it contains,
            all processing is done directly in your browser so the image is not transmitted
          </p>

          <p class="mb-0">
            Exif data may be found in compressed JPEG images and may contain various pieces of information such as
            GPS data, lens information or camera setting information
          </p>
        </div>
      </b-col>
    </b-row>

    <hr />

    <b-row>
      <b-col>
        <div class="text-center">
          <div v-if="loading">
            <b-spinner class="spinner m-5" variant="light" label="Loading..."></b-spinner>
          </div>
        </div>

        <b-form-file
          v-if="!Boolean(file)"
          v-model="file"
          placeholder="Choose a file..."
          drop-placeholder="Drop file here..."
          @change="parseMeta"
        ></b-form-file>
      </b-col>
    </b-row>

    <b-row>
      <b-col>
        <b-img v-if="Boolean(file)" class="m-3" fluid center :src="image"></b-img>
      </b-col>
    </b-row>

    <hr v-if="Boolean(file)" />

    <div class="metaInformation text-center" v-if="metadata !== null">
      <b-row>
        <b-col>
          <h3 class="text-center">Exif Data</h3>
        </b-col>
      </b-row>

      <b-row>

        <b-col cols="6">
          <h5>
            <u>File Info</u>
          </h5>

          <div v-if="metadata.DateTimeOriginal != undefined" class="group">
            <div class="key">Photo taken:</div>
            <div class="value">{{metadata.DateTimeOriginal}}</div>
          </div>

          <div v-if="metadata.ModifyDate != undefined" class="group">
            <div class="key">Modified Date:</div>
            <div class="value">{{metadata.ModifyDate}}</div>
          </div>

          <div v-if="metadata.Software != undefined" class="group">
            <div class="key">Software:</div>
            <div class="value">{{metadata.Software}}</div>
          </div>

          <div v-if="metadata.Artist != undefined" class="group">
            <div class="key">Artist:</div>
            <div class="value">{{metadata.Artist}}</div>
          </div>

          <div v-if="metadata.Copyright != undefined" class="group">
            <div class="key">Copyright:</div>
            <div class="value">{{metadata.Copyright}}</div>
          </div>

        </b-col>

        <b-col cols="6">
          <h5>
            <u>Exposure Info</u>
          </h5>

          <div v-if="metadata.ExposureTime != undefined" class="group">
            <div class="key">Shutter Speed:</div>
            <div class="value">{{metadata.ExposureTime}} sec</div>
          </div>

          <div v-if="metadata.FNumber != undefined" class="group">
            <div class="key">Aperture:</div>
            <div class="value">{{metadata.FNumber}}F</div>
          </div>

          <div v-if="metadata.ISO != undefined" class="group">
            <div class="key">ISO:</div>
            <div class="value">{{metadata.ISO}}</div>
          </div>

          <div v-if="metadata.ExposureMode != undefined" class="group">
            <div class="key">Exposure Mode:</div>
            <div class="value">{{metadata.ExposureMode}}</div>
          </div>

          <div v-if="metadata.ExposureProgram != undefined" class="group">
            <div class="key">Exposure Program:</div>
            <div class="value">{{metadata.ExposureProgram}}</div>
          </div>

          <div v-if="metadata.ExposureCompensation != undefined" class="group">
            <div class="key">Exposure Compensation:</div>
            <div class="value">{{metadata.ExposureCompensation}}</div>
          </div>
          
        </b-col>

        <b-col cols="6">
          <h5>
            <u>Camera &amp; Lens information</u>
          </h5>

          <div v-if="metadata.OwnerName != undefined" class="group">
            <div class="key">Body Owner Name:</div>
            <div class="value">{{metadata.OwnerName}}</div>
          </div>

          <div v-if="metadata.Make != undefined" class="group">
            <div class="key">Camera Make:</div>
            <div class="value">{{metadata.Make}}</div>
          </div>

          <div v-if="metadata.Model != undefined" class="group">
            <div class="key">Camera Model:</div>
            <div class="value">{{metadata.Model}}</div>
          </div>

          <div v-if="metadata.LensMake != undefined" class="group">
            <div class="key">Lens Make:</div>
            <div class="value">{{metadata.LensMake}}</div>
          </div>

          <div v-if="metadata.LensModel != undefined" class="group">
            <div class="key">Lens Model:</div>
            <div class="value">{{metadata.LensModel}}</div>
          </div>

          <div v-if="metadata.SerialNumber != undefined" class="group">
            <div class="key">Body Serial Number:</div>
            <div class="value">{{metadata.SerialNumber}}</div>
          </div>

          <div v-if="metadata.LensSerialNumber != undefined" class="group">
            <div class="key">Lens Serial Number:</div>
            <div class="value">{{metadata.LensSerialNumber}}</div>
          </div>
          
        </b-col>

        <b-col cols="6" v-if="metadata.GPSLatitude != undefined">
          <h5>
            <u>GPS Info</u>
          </h5>

          <div v-if="metadata.GPSVersionID != undefined" class="group">
            <div class="key">Version ID:</div>
            <div class="value">{{metadata.GPSVersionID}}</div>
          </div>

          <div v-if="metadata.GPSLatitude != undefined && metadata.GPSLatitudeRef != undefined" class="group">
            <div class="key">Latitude:</div>
            <div class="value">{{metadata.GPSLatitude}} {{metadata.GPSLatitudeRef}}</div>
          </div>

          <div v-if="metadata.GPSLongitude != undefined && metadata.GPSLongitudeRef != undefined" class="group">
            <div class="key">Longitude:</div>
            <div class="value">{{metadata.GPSLongitude}} {{metadata.GPSLongitudeRef}}</div>
          </div>

          <a 
          v-if="metadata.GPSLatitude != undefined && metadata.GPSLatitudeRef != undefined
            && metadata.GPSLongitude != undefined && metadata.GPSLongitudeRef != undefined"
          :href="'https://www.google.com/maps/search/?api=1&query=' + metadata.GPSLatitude + ' ' + metadata.GPSLatitudeRef + ', ' + metadata.GPSLongitude + ' ' + metadata.GPSLongitudeRef"
          onmousedown="event.preventDefault()"
          target="_blank"
          class="btn btn-default my-2"
          >
            <i class="fas fa-map-marker-alt fa-fw"></i>
            <span class="button-text">View on map</span>
          </a>

          <div v-if="metadata.GPSAltitudeRef != undefined" class="group">
            <div class="key">Altitude:</div>
            <div class="value">{{metadata.GPSAltitudeRef}}</div>
          </div>

        </b-col>


      </b-row>
    </div>


      <b-row>
        <b-col>
          <div class="text-center pt-2">
          <b-btn
          v-if="Boolean(file)"
          @click="reset"
          target="_blank"
          class="btn btn-default"
          >
            <i class="fas fa-redo fa-fw pr-3"></i>
            <span class="button-text">reset</span>
          </b-btn>
          </div>
        </b-col>
      </b-row>

    <hr />
  </b-container>
</template>

<script>
import ImageMeta from "image-meta-js";

export default {
  name: "Meta",
  data: function() {
    return {
      loading: false,
      metadata: null,
      file: null,
      image: null,
    };
  },
  methods: {
    parseMeta(event) {
      this.loading = true;
      let reader = new FileReader();

      reader.readAsArrayBuffer(event.target.files[0]);
      reader.onload = () => {
        // Lots of type shuffling
        let arrayBuffer = reader.result;
        let binary = new DataView(arrayBuffer);

        // Load the image into the library
        let meta = new ImageMeta(binary, true);
        let data = meta.data();
        this.metadata = meta.data();

        this.loadImagePreview(event.target.files[0]);
      };
    },
    loadImagePreview(image) {
      let imageReader = new FileReader();
      imageReader.readAsDataURL(image);

      imageReader.onload = () => {
        this.image = imageReader.result;
        this.loading = false; //cancel loading icon
      };
    },
    reset() {
      this.metadata = null,
      this.image = null,
      this.file = null
    }
  },
  
};
</script>

<style scoped>
img {
  max-width: 60%;
}

.spinner {
  width: 6rem;
  height: 6rem;
}

u {
  text-underline-position: under;
}

.group {
  color: white;
  padding-bottom: 5px;
}

.key {
  font-weight: 200;
  font-size: 0.9rem;
  padding-right: 5px;
  display: inline-block;
}

.value {
  font-weight: 600;
  display: inline-block;
}

.metaInformation [class*='col-'] {
  padding-top:20px;
}

</style>
