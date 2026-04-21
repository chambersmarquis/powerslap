<script lang="ts">
  // Import base path helper for resolving image paths correctly in SvelteKit
  import { base } from '$app/paths';
</script>

<!-- ============================================================
     BYLINE COMPONENT
     Displays two circular author photos + name + affiliation + date
     ============================================================ -->

<div class="byline container">

  <!-- Bootstrap row: centers content horizontally -->
  <div class="row justify-content-center">

    <!-- CUSTOMIZE: Adjust column breakpoints to control content width
         col-12    = full width on mobile
         col-sm-10 = 10/12 on small screens
         col-lg-8  = 8/12 on large screens
         col-xxl-6 = 6/12 on extra-large screens -->
    <div class="col-12 col-sm-10 col-lg-8 col-xxl-6">

      <!-- Outer flex row: holds photo group + text block side by side -->
      <div class="d-flex align-items-center gap-3">

        <!-- ── AUTHOR PHOTOS ──────────────────────────────────── -->
        <!-- Flex wrapper groups both circle images together -->
        <div class="author-photos">

          <!-- PHOTO 1: Marquis
               CUSTOMIZE: Change src filename to swap photo -->
          <img
            src="{base}/images/marquis.jpeg"
            alt="Marquis"
            class="author-photo photo-marquis"
          />

          <!-- PHOTO 2: Jennifer
               CUSTOMIZE: Change src filename to swap photo -->
          <img
            src="{base}/images/jennifer.jpeg"
            alt="Jennifer"
            class="author-photo photo-jennifer"
          />

        </div>
        <!-- ── END AUTHOR PHOTOS ──────────────────────────────── -->

        <!-- ── BYLINE TEXT ────────────────────────────────────── -->
        <!-- Stacks author name, affiliation, and date vertically -->
        <div class="d-flex flex-column justify-content-center">

          <!-- Author names
               CUSTOMIZE: Update names as needed -->
          <strong class="author-names">By Marquis and Jennifer</strong>
          <div class="affiliation">2026 Graduate School of Journalism</div>

  

        </div>
        <!-- ── END BYLINE TEXT ───────────────────────────────── -->

      </div>
    </div>
  </div>
</div>


<style>
  /* ============================================================
     BYLINE WRAPPER
     CUSTOMIZE: font-family | font-size | margin-top | padding-bottom
     ============================================================ */
  .byline {
    font-family: Arial, Helvetica, sans-serif; /* CUSTOMIZE: swap for any font stack */
    font-size: 1.1rem;
    font-weight: 400;
    margin-top: 0rem;       /* CUSTOMIZE: space above byline from the content above */
    padding-bottom: 1rem;   /* CUSTOMIZE: space below byline before next section */
  }

  /* ============================================================
     PHOTO WRAPPER
     Controls spacing between the two circle images
     CUSTOMIZE: gap — use positive px for space, negative px for overlap
     ============================================================ */
  .author-photos {
    display: flex;           /* Lays both photos out in a horizontal row */
    align-items: center;     /* Vertically centers photos if they differ in size */
    gap: 6px;               /* CUSTOMIZE: space between photos (try -10px for overlap effect) */
    flex-shrink: 0;          /* Prevents photos from shrinking when the window narrows */
  }

  /* ============================================================
     INDIVIDUAL PHOTO — shared styles applied to both circles
     CUSTOMIZE: width/height = diameter | border = ring around photo
     ============================================================ */
  .author-photo {
    width: 55px;             /* CUSTOMIZE: photo diameter */
    height: 55px;            /* CUSTOMIZE: must match width to keep the circle shape */
    border-radius: 50%;      /* Makes the square image a perfect circle */
    object-fit: cover;       /* Crops image to fill circle without stretching */
    object-position: center; /* CUSTOMIZE: "top" | "bottom" | "50% 20%" to reframe face */
    border: 2px solid #fff;  /* CUSTOMIZE: ring color between photos — set to "none" to remove */
    display: block;
  }

  /* ============================================================
     PHOTO-SPECIFIC OVERRIDES
     Use if one photo needs a different crop position or size
     ============================================================ */
  .photo-marquis {
    /* CUSTOMIZE: e.g. object-position: top; if face sits low in the image */
  }

  .photo-jennifer {
    /* CUSTOMIZE: e.g. object-position: 50% 15%; if face is near the top */
  }

  /* ============================================================
     AUTHOR NAMES ("By Marquis and Jennifer")
     CUSTOMIZE: font-size | font-weight | color
     ============================================================ */
  .author-names {
    font-size: 1rem;         /* CUSTOMIZE: size of the author name line */
    font-weight: 600;        /* CUSTOMIZE: boldness — 400 normal, 600 semi-bold, 700 bold */
    line-height: 1.3;        /* CUSTOMIZE: spacing between lines if name wraps */
  }

  /* ============================================================
     AFFILIATION (school or outlet name)
     CUSTOMIZE: font-size | opacity (lower = more faded/muted)
     ============================================================ */
  .affiliation {
    font-size: 0.95rem;      /* CUSTOMIZE: slightly smaller than the author name */
    opacity: 0.6;            /* CUSTOMIZE: 1.0 = full color | 0.4 = more subtle */
    line-height: 1.3;
  }

  /* ============================================================
     PUBLICATION DATE
     CUSTOMIZE: font-size | color | margin-top (space above date)
     ============================================================ */
  .pub-date {
    font-size: 0.9rem;       /* CUSTOMIZE: date text size */
    font-weight: 300;        /* CUSTOMIZE: light weight keeps date subtle */
    color: grey;             /* CUSTOMIZE: any CSS color — e.g. #999 or var(--your-color) */
    margin: 0.2rem 0 0;      /* CUSTOMIZE: top gap between affiliation and date */
  }
</style>